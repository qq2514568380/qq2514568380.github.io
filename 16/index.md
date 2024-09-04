# 策略模式的具体实现


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# 设计模型具体实现

### 1. **支付方式选择**

**场景**：在电商应用中，用户可以选择不同的支付方式（如信用卡、PayPal、比特币）。

#### 实现



```java
// 策略接口
interface PaymentStrategy {
    void pay(int amount);
}

// 具体策略：信用卡支付
class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card.");
    }
}

// 具体策略：PayPal支付
class PayPalPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal.");
    }
}

// 上下文
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}
```

#### 使用示例

java

```java
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        
        cart.setPaymentStrategy(new CreditCardPayment());
        cart.checkout(100);

        cart.setPaymentStrategy(new PayPalPayment());
        cart.checkout(200);
    }
}
```

### 2. **排序算法选择**

**场景**：在应用中，用户可以选择不同的排序算法（如快速排序、冒泡排序）。

#### 实现

java

```java
// 策略接口
interface SortStrategy {
    void sort(int[] array);
}

// 具体策略：快速排序
class QuickSort implements SortStrategy {
    public void sort(int[] array) {
        // 快速排序实现
        System.out.println("QuickSort applied.");
    }
}

// 具体策略：冒泡排序
class BubbleSort implements SortStrategy {
    public void sort(int[] array) {
        // 冒泡排序实现
        System.out.println("BubbleSort applied.");
    }
}

// 上下文
class SortContext {
    private SortStrategy sortStrategy;

    public void setSortStrategy(SortStrategy sortStrategy) {
        this.sortStrategy = sortStrategy;
    }

    public void sortArray(int[] array) {
        sortStrategy.sort(array);
    }
}
```

#### 使用示例

java

```
public class Main {
    public static void main(String[] args) {
        SortContext context = new SortContext();
        int[] array = {5, 3, 8, 1};

        context.setSortStrategy(new QuickSort());
        context.sortArray(array);

        context.setSortStrategy(new BubbleSort());
        context.sortArray(array);
    }
}
```

### 3. **文件压缩工具**

**场景**：用户可以选择不同的文件压缩算法（如 ZIP、RAR）。

#### 实现

java

```java
// 策略接口
interface CompressionStrategy {
    void compress(String fileName);
}

// 具体策略：ZIP压缩
class ZipCompression implements CompressionStrategy {
    public void compress(String fileName) {
        System.out.println("Compressing " + fileName + " using ZIP.");
    }
}

// 具体策略：RAR压缩
class RarCompression implements CompressionStrategy {
    public void compress(String fileName) {
        System.out.println("Compressing " + fileName + " using RAR.");
    }
}

// 上下文
class CompressionContext {
    private CompressionStrategy compressionStrategy;

    public void setCompressionStrategy(CompressionStrategy compressionStrategy) {
        this.compressionStrategy = compressionStrategy;
    }

    public void compressFile(String fileName) {
        compressionStrategy.compress(fileName);
    }
}
```

#### 使用示例

java

```java
public class Main {
    public static void main(String[] args) {
        CompressionContext context = new CompressionContext();
        
        context.setCompressionStrategy(new ZipCompression());
        context.compressFile("file1.txt");

        context.setCompressionStrategy(new RarCompression());
        context.compressFile("file2.txt");
    }
}
```

### 4. **旅行路径选择**

**场景**：用户选择不同的旅行策略（如驾车、步行、骑行）。

#### 实现

java

```java
// 策略接口
interface TravelStrategy {
    void travel(String start, String destination);
}

// 具体策略：驾车
class CarTravel implements TravelStrategy {
    public void travel(String start, String destination) {
        System.out.println("Traveling by car from " + start + " to " + destination);
    }
}

// 具体策略：步行
class WalkingTravel implements TravelStrategy {
    public void travel(String start, String destination) {
        System.out.println("Walking from " + start + " to " + destination);
    }
}

// 上下文
class TravelContext {
    private TravelStrategy travelStrategy;

    public void setTravelStrategy(TravelStrategy travelStrategy) {
        this.travelStrategy = travelStrategy;
    }

    public void travel(String start, String destination) {
        travelStrategy.travel(start, destination);
    }
}
```

#### 使用示例

java

```java
public class Main {
    public static void main(String[] args) {
        TravelContext context = new TravelContext();
        
        context.setTravelStrategy(new CarTravel());
        context.travel("Home", "Office");

        context.setTravelStrategy(new WalkingTravel());
        context.travel("Home", "Park");
    }
}
```
