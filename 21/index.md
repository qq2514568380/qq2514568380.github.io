# parallelstream的工作原理


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# parallelstream的工作原理

**底层实现原理：**

1. **ForkJoinPool 的使用：**
   - `parallelStream()` 默认使用 `ForkJoinPool.commonPool()` 作为其执行线程池。
   - `commonPool` 的线程数通常为 `Runtime.getRuntime().availableProcessors() - 1`，即 CPU 核心数减一。
2. **数据拆分：**
   - 在并行流的操作中，数据源会被拆分成多个子任务。
   - 拆分策略通常是递归的，直到子任务足够小，适合单个线程处理。
3. **合并结果：**
   - 各个子任务完成后，其结果会被合并。
   - 合并操作通常是归约操作，如求和、求平均等。

**性能考虑：**

- **线程池大小：**

  - 默认情况下，`ForkJoinPool.commonPool()` 的线程数为 CPU 核心数减一。

  - 可以通过设置系统属性 `java.util.concurrent.ForkJoinPool.common.parallelism` 来调整线程池的大小。

  - 例如，设置为 4：

    ```java
    System.setProperty("java.util.concurrent.ForkJoinPool.common.parallelism", "4");
    ```

  - 需要注意的是，修改此属性会影响所有使用 `commonPool` 的并行流和其他并行任务。

- **任务拆分与合并开销：**

  - 并行处理带来了拆分和合并的开销。
  - 对于小数据量或简单操作，使用并行流可能反而导致性能下降。
  - 建议在数据量较大且操作复杂时使用并行流，以获得性能提升。

- **线程安全：**

  - 避免在并行流中修改共享的可变状态，以防止并发问题。

**自定义线程池：**

如果需要使用自定义的线程池，可以通过 `ForkJoinPool` 创建并传递给并行流：

```java
ForkJoinPool customThreadPool = new ForkJoinPool(4);
customThreadPool.submit(() -> {
    list.parallelStream().forEach(item -> {
        // 处理逻辑
    });
}).get();
customThreadPool.shutdown();
```

需要注意的是，使用自定义线程池时，可能需要手动管理线程池的生命周期。

**总结：**

`parallelStream()` 通过 `ForkJoinPool` 实现数据的并行处理。 在使用时，需要考虑线程池的配置、任务拆分与合并的开销，以及线程安全等因素，以确保获得最佳的性能提升。


