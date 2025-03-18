# adapter设计模式


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# adapter设计模式

适配器设计模式（Adapter Pattern）是一种结构型设计模式，主要用于解决不同接口之间的兼容性问题。它通过创建一个适配器类，将一个类的接口转换成客户端所期待的另一种接口，从而使原本由于接口不兼容而无法一起工作的类可以协同工作。

### 主要组成部分

1. **目标接口（Target Interface）**：
   - 客户端所期待的接口。
2. **源类（Adaptee）**：
   - 需要适配的现有类，具有一个与目标接口不兼容的接口。
3. **适配器类（Adapter）**：
   - 实现目标接口，内部持有源类的实例，通过调用源类的方法来实现目标接口的方法。

### 使用场景

- 当你想使用一个已有的类，但其接口不符合你的需求时。
- 当你需要创建一个可与多个不同的接口一起工作的类时。

### 示例

以下是一个简单的 Java 示例，展示了适配器模式的用法：

java

复制

```
// 目标接口
interface Target {
    void request();
}

// 源类
class Adaptee {
    void specificRequest() {
        System.out.println("Called specificRequest()");
    }
}

// 适配器类
class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        // 调用源类的方法
        adaptee.specificRequest();
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request();  // 输出: Called specificRequest()
    }
}
```

### 优点

- **解耦**：适配器模式将接口的实现与使用分离，降低了代码的耦合度。
- **复用性**：可以复用现有的类，只需通过适配器进行适配。
- **灵活性**：可以在不修改源类的情况下，适配不同的接口。

### 缺点

- **增加复杂性**：增加了一个适配器类，可能导致系统复杂度上升。
- **性能开销**：适配器模式可能引入一些性能开销，尤其是在大量适配的情况下。

适配器模式是一种非常实用的设计模式，能够帮助开发者在面对接口不兼容问题时提供灵活的解决方案。
