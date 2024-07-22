CountDownLatch是 Java 中提供的一种同步辅助工具类，用于协调多个线程之间的同步。它允许一个或多个线程等待其他线程完成一组操作。
### 1. 基本概念
CountDownLatch通过一个计数器来实现。计数器的初始值为线程的数量。每当一个线程完成了自己的任务后，计数器的值就减一。当计数器的值变为零时，表示所有线程都已经完成了任务，等待的线程就可以继续执行了。
### 2. 构造方法
```
public CountDownLatch(int count)
```

- count：计数器的初始值，通常设置为需要等待的线程数量。
### 3. 主要方法

- void await() throws InterruptedException：使当前线程等待，直到计数器的值变为零。
- boolean await(long timeout, TimeUnit unit) throws InterruptedException：使当前线程等待，直到计数器的值变为零或者等待时间超过指定的时间。
- void countDown()：将计数器的值减一。
### 4. 使用示例
#### 示例 1：等待多个线程完成任务
假设我们有一个主线程，它需要等待多个工作线程完成任务后再继续执行。
```
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample {
    public static void main(String[] args) {
        final int threadCount = 3;
        final CyclicBarrier barrier = new CyclicBarrier(threadCount, new Runnable() {
            @Override
            public void run() {
                System.out.println("All parties have arrived at the barrier, let's continue.");
            }
        });

        for (int i = 0; i < threadCount; i++) {
            new Thread(new Task(barrier)).start();
        }
    }
}

class Task implements Runnable {
    private final CyclicBarrier barrier;

    public Task(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            System.out.println(Thread.currentThread().getName() + " is working on part 1...");
            Thread.sleep(1000); // 模拟任务部分1
            System.out.println(Thread.currentThread().getName() + " has finished part 1.");
            
            barrier.await(); // 等待其他线程完成部分1

            System.out.println(Thread.currentThread().getName() + " is working on part 2...");
            Thread.sleep(1000); // 模拟任务部分2
            System.out.println(Thread.currentThread().getName() + " has finished part 2.");
        } catch (InterruptedException | BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
```
在这个示例中，主线程创建了 3 个工作线程，并使用CountDownLatch来等待它们完成任务。所有工作线程完成任务后，主线程才会继续执行。
#### 示例 2：模拟并行启动多个线程
假设我们有多个线程，它们需要在同一时间点开始执行任务。
```
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample2 {
    public static void main(String[] args) {
        final int threadCount = 3;
        final CyclicBarrier barrier = new CyclicBarrier(threadCount, new Runnable() {
            @Override
            public void run() {
                System.out.println("All parties have arrived at the barrier, let's continue.");
            }
        });

        for (int i = 0; i < threadCount; i++) {
            new Thread(new ReusableTask(barrier)).start();
        }
    }
}

class ReusableTask implements Runnable {
    private final CyclicBarrier barrier;

    public ReusableTask(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            for (int i = 0; i < 3; i++) { // 重复3次
                System.out.println(Thread.currentThread().getName() + " is working on iteration " + (i + 1) + "...");
                Thread.sleep(1000); // 模拟任务
                System.out.println(Thread.currentThread().getName() + " has finished iteration " + (i + 1) + ".");
                
                barrier.await(); // 等待其他线程完成当前迭代
            }
        } catch (InterruptedException | BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierExample2 {
    public static void main(String[] args) {
        final int threadCount = 3;
        final CyclicBarrier barrier = new CyclicBarrier(threadCount, new Runnable() {
            @Override
            public void run() {
                System.out.println("All parties have arrived at the barrier, let's continue.");
            }
        });

        for (int i = 0; i < threadCount; i++) {
            new Thread(new ReusableTask(barrier)).start();
        }
    }
}

class ReusableTask implements Runnable {
    private final CyclicBarrier barrier;

    public ReusableTask(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            for (int i = 0; i < 3; i++) { // 重复3次
                System.out.println(Thread.currentThread().getName() + " is working on iteration " + (i + 1) + "...");
                Thread.sleep(1000); // 模拟任务
                System.out.println(Thread.currentThread().getName() + " has finished iteration " + (i + 1) + ".");
                
                barrier.await(); // 等待其他线程完成当前迭代
            }
        } catch (InterruptedException | BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
```
在这个示例中，readyLatch用于等待所有线程准备就绪，而startLatch用于发出开始信号。当所有线程准备就绪后，startLatch的计数器减一，所有线程同时开始执行任务。
### 5. 总结
CountDownLatch是一种用于协调多个线程之间同步的工具类。它通过一个计数器来控制线程的执行顺序，适用于需要等待一组线程完成任务或者需要同时启动多组线程的场景。
