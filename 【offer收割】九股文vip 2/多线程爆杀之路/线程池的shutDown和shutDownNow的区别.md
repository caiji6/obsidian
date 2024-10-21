# 👌线程池的shutDown和shutDownNow的区别

# 题目详细答案
ExecutorService接口中，shutdown()和shutdownNow()是用于关闭线程池的方法。

## shutdown()
shutdown()方法会启动线程池的关闭过程。它会停止接受新的任务提交，但会继续执行已经提交的任务（包括正在执行的和已提交但尚未开始执行的任务）。调用shutdown()后，线程池会进入一个平滑的关闭过程，等待所有已提交的任务完成后才会完全终止。

```plain
ExecutorServiceexecutorService= Executors.newFixedThreadPool(5);
// 提交一些任务for (inti=0; i < 10; i++) {
    executorService.submit(() -> {
        System.out.println("Task executed by " + Thread.currentThread().getName());
    });
}
// 调用shutdown()
executorService.shutdown();
```

## shutdownNow()
shutdownNow()方法会尝试停止所有正在执行的任务，并返回一个包含尚未开始执行的任务的列表。它会立即停止接收新的任务，并试图中断正在执行的任务。调用shutdownNow()后，线程池会尽快停止所有正在执行的任务，并返回尚未开始执行的任务列表。需要注意的是，无法保证所有正在执行的任务都能被中断。

```plain
ExecutorServiceexecutorService= Executors.newFixedThreadPool(5);
// 提交一些任务for (inti=0; i < 10; i++) {
    executorService.submit(() -> {
        System.out.println("Task executed by " + Thread.currentThread().getName());
        try {
            Thread.sleep(1000); // 模拟长时间运行的任务
        } catch (InterruptedException e) {
            System.out.println("Task interrupted");
        }
    });
}
// 调用shutdownNow()
List<Runnable> notExecutedTasks = executorService.shutdownNow();
System.out.println("Tasks not executed: " + notExecutedTasks.size());
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/godm0x4u5gcnbsoo>