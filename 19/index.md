# kafka Stream实战


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# kafka Stream实战

## 1.要求

某车企通过将车辆传感器数据实时发送到服务器，再由 Kafka 作为数据总线接收这些数据,再调用第三方API 实现数据处理;

- 要求:QPS>5w

## 2.实战

在 Kubernetes 集群上，我们部署了 Kafka Streams。最初的部署配置为 8 个 Pod，同时 Kafka Topic 的分区数设置为 36，此时系统的 QPS 达到约 7000+。为进一步提高并行处理能力和整体吞吐量，我们对 Kafka Topic 进行了优化，将分区数扩展到 102 个。经过调整后，系统的 QPS 显著提升至约 25,000+。但是还没达到5w 后续考虑 改回kafka consumer/producer + 异步消费的方式
