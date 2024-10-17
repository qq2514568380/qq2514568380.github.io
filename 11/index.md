# kafka在分布式系统中的使用


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# Kafka在分布式系统中的使用

在分布式系统中，Kafka Streams 的强大之处在于其能够高效地处理跨多个节点的数据流。下面我们来构建一个更复杂的 **分布式流处理** 场景，并以**订单处理系统**为例展示如何使用 Kafka Streams 实现分布式处理。

### 场景描述

假设我们有一个电商系统，它分布式地处理用户的订单流。订单数据流不断涌入 Kafka 主题。我们希望在分布式环境下，能够实时处理这些订单，执行以下几步：

1. **订单过滤**：只处理金额大于 100 元的订单。
2. **订单分类**：根据订单的商品类型将订单分类。
3. **统计处理**：每 5 分钟内统计每种商品类型的订单数量，并进行实时更新。
4. **结果输出**：将统计结果输出到 Kafka 中的一个新的主题。

### 分布式处理架构

该流处理任务将在多个实例上运行，每个实例可以处理一部分数据。Kafka Streams 的内部机制会确保数据被合理分配到各个实例上，实现分布式的无缝处理。

### 1. Kafka Topics

- **订单输入主题 (orders)**：包含所有用户下单的数据流，每条记录的值是订单信息。

  ```
  
  {
    "orderId": "12345",
    "productType": "electronics",
    "amount": 150.0
  }
  ```

- **订单统计输出主题 (order_statistics)**：存储每个商品类型的实时订单统计信息。

  ```
  
  {
    "productType": "electronics",
    "count": 50,
    "windowStart": "2024-10-17T10:00:00",
    "windowEnd": "2024-10-17T10:05:00"
  }
  ```

### 2. Kafka Streams 拓扑设计

流处理拓扑包含以下几个步骤：

1. **读取 Kafka 订单主题**。
2. **过滤订单**，只保留金额大于 100 元的订单。
3. **根据商品类型进行分组**。
4. **基于 5 分钟的时间窗口进行统计**。
5. **将统计结果写回 Kafka 输出主题**。

### 3. 实现代码

以下是一个分布式订单处理系统的 Kafka Streams 代码示例：

```java

import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.StreamsBuilder;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.*;
import org.apache.kafka.streams.state.Stores;

import java.time.Duration;
import java.util.Properties;

public class DistributedOrderProcessing {

    public static void main(String[] args) {

        // Kafka Streams 配置
        Properties props = new Properties();
        props.put(StreamsConfig.APPLICATION_ID_CONFIG, "order-processing-app");
        props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass());
        props.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass());

        // 构建流处理拓扑
        StreamsBuilder builder = new StreamsBuilder();

        // 读取 "orders" 主题
        KStream<String, String> orderStream = builder.stream("orders");

        // 过滤订单金额大于 100 的订单
        KStream<String, String> filteredOrders = orderStream.filter((key, value) -> {
            // 假设订单值是 JSON 字符串，将其解析为对象并判断金额
            Order order = parseOrder(value);
            return order.getAmount() > 100;
        });

        // 按商品类型分组
        KGroupedStream<String, String> groupedByProductType = filteredOrders.groupBy((key, value) -> {
            Order order = parseOrder(value);
            return order.getProductType();
        });

        // 基于 5 分钟的窗口进行统计
        TimeWindows timeWindows = TimeWindows.of(Duration.ofMinutes(5));
        KTable<Windowed<String>, Long> productTypeCounts = groupedByProductType
                .windowedBy(timeWindows)
                .count(Materialized.as(Stores.persistentWindowStore("product-type-counts", Duration.ofMinutes(5), Duration.ofMinutes(1), false)));

        // 将统计结果写入到 "order_statistics" 主题
        productTypeCounts.toStream().map((key, value) -> {
            String productType = key.key();
            String windowStart = key.window().startTime().toString();
            String windowEnd = key.window().endTime().toString();
            return KeyValue.pair(productType, String.format("{\"productType\":\"%s\",\"count\":%d,\"windowStart\":\"%s\",\"windowEnd\":\"%s\"}", productType, value, windowStart, windowEnd));
        }).to("order_statistics", Produced.with(Serdes.String(), Serdes.String()));

        // 创建 Kafka Streams 实例
        KafkaStreams streams = new KafkaStreams(builder.build(), props);

        // 启动流处理应用
        streams.start();

        // 添加关闭 hook
        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }

    // 解析订单数据（假设订单是 JSON 格式）
    private static Order parseOrder(String value) {
        // 此处省略具体的 JSON 解析逻辑
        // 返回一个 Order 对象
        return new Order("12345", "electronics", 150.0);
    }

    // 模拟订单类
    static class Order {
        private String orderId;
        private String productType;
        private double amount;

        public Order(String orderId, String productType, double amount) {
            this.orderId = orderId;
            this.productType = productType;
            this.amount = amount;
        }

        public String getOrderId() { return orderId; }
        public String getProductType() { return productType; }
        public double getAmount() { return amount; }
    }
}
```

### 4. 分布式处理

在这个示例中，Kafka Streams 的分布式特性能够自动将处理任务分布到不同的实例上。当我们在多个机器或多个 JVM 实例上运行这个应用时，Kafka Streams 会自动：

- **分配分区**：根据 Kafka 主题分区将不同的分区数据分配给不同的实例处理。
- **故障恢复**：如果某个实例失败，Kafka Streams 会自动将失败实例的任务重新分配给其他活跃的实例。
- **状态存储**：对于有状态的操作（如 `count()`），Kafka Streams 使用内部的存储机制（如 RocksDB）来保持处理状态，这些状态也可以被持久化到 Kafka 中，以便在故障时恢复。


