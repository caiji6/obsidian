# 👌介绍一下常用的java的线程池？

# 题目详细答案
## FixedThreadPool（固定大小线程池）
线程池中有固定数量的线程。无论有多少任务提交，线程池中的线程数量始终不变。当所有线程都处于忙碌状态时，新的任务将会在队列中等待。适用于负载较均衡的场景，任务数量相对稳定。

```plain
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(int nThreads);
```

## CachedThreadPool（缓存线程池）
线程池中线程数量不固定，可以根据需要自动创建新线程。如果线程池中的线程在60秒内没有被使用，则会被终止并从池中移除。当提交新任务时，如果没有空闲线程，则会创建新线程。适用于执行很多短期异步任务的小程序，或者负载较轻的服务器。

```plain
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
```

## SingleThreadExecutor（单线程池）
线程池中只有一个线程，所有任务按照提交的顺序执行。确保所有任务在同一个线程中按顺序执行。适用于需要保证顺序执行任务的场景。

```plain
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
```

## ScheduledThreadPool（定时线程池）
线程池可以在给定延迟后运行任务，或者定期执行任务。类似于Timer类，但更灵活且功能更强大。适用于需要周期性执行任务的场景。

```plain
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(int corePoolSize);
```

## WorkStealingPool（工作窃取线程池）
使用多个工作队列减少竞争，适用于并行计算。线程池中的线程数量是Runtime.getRuntime().availableProcessors()的返回值。适用于需要大量并行任务的场景。

```plain
ExecutorService workStealingPool = Executors.newWorkStealingPool();
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/zcuxm2pogymgs7g6>