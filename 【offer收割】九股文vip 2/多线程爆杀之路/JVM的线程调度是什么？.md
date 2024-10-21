# 👌JVM的线程调度是什么？

# 题目详细答案
线程调度是指 JVM 如何管理和分配 CPU 时间给各个线程。线程调度策略直接影响到多线程程序的性能和响应性。JVM 的线程调度主要依赖于底层操作系统的线程调度机制。

## 线程调度的基本概念
线程调度是指 JVM 选择某个线程来获得 CPU 时间片的过程。分为两种类型：

1. **抢占式调度**：操作系统根据线程的优先级和其他因素，强制中断正在运行的线程，并将 CPU 分配给另一个线程。
2. **协作式调度**：线程主动放弃 CPU 使用权，操作系统不会强制中断线程。

通常使用抢占式调度。

## JVM 和操作系统的关系
JVM 的线程调度依赖于操作系统的调度器。JVM 将 Java 线程映射到操作系统的本地线程，由操作系统负责实际的线程调度。因此，JVM 的线程调度行为在很大程度上取决于底层操作系统。

## 线程调度策略
#### 时间片轮转
在时间片轮转调度策略中，每个线程按顺序获得一个固定长度的时间片，当时间片用完时，调度器将 CPU 分配给下一个线程。这种方式确保每个线程都有机会运行。

#### 优先级调度
在优先级调度策略中，调度器优先选择高优先级线程运行。如果有多个同等优先级的线程，则使用时间片轮转策略。

## 线程调度 Demo
```plain
public class ThreadSchedulingExample {
    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getName() + " is running");
                try {
                    Thread.sleep(100); // 模拟工作
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        };

        Thread t1 = new Thread(task, "Thread-1");
        Thread t2 = new Thread(task, "Thread-2");

        t1.setPriority(Thread.MIN_PRIORITY);
        t2.setPriority(Thread.MAX_PRIORITY);

        t1.start();
        t2.start();
    }
}
```

Thread-2有更高的优先级，它更有可能获得 CPU 时间片。

## 常见的线程调度问题
#### 线程饥饿
线程饥饿发生在低优先级线程长时间得不到 CPU 时间片的情况下。这可能是由于高优先级线程频繁占用 CPU 资源所致。

#### 活锁
活锁类似于死锁，但线程并未真正阻塞，而是不断地改变状态，无法继续执行。活锁通常发生在线程频繁响应彼此的动作时。

## 线程调度优化
+ **合理设置线程优先级**：避免所有线程都设置为最高优先级，合理分配优先级以避免饥饿问题。
+ **减少锁竞争**：尽量减少线程之间的锁竞争，使用更细粒度的锁或无锁数据结构。
+ **使用线程池**：通过java.util.concurrent包中的线程池来管理线程，避免频繁创建和销毁线程带来的开销。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gc9kt8segeue3awn>