# 👌CyclicBarrier用法

# 题目详细答案
CyclicBarrier是 Java 提供的另一种同步辅助工具类，用于协调多个线程在某个特定点上同步。与CountDownLatch不同，CyclicBarrier可以被重复使用。

## 基本概念
CyclicBarrier允许一组线程相互等待，直到所有线程都到达某个公共的屏障点（barrier）。当所有线程都到达屏障点时，屏障就会被打开，所有线程继续执行。

## 构造方法
```plain
public CyclicBarrier(int parties)
public CyclicBarrier(int parties, Runnable barrierAction)
```

parties：需要等待的线程数量。

barrierAction：一个可选的Runnable，在所有线程到达屏障点后执行。

## 主要方法
int await() throws InterruptedException, BrokenBarrierException：使当前线程等待，直到所有线程都到达屏障点。

int await(long timeout, TimeUnit unit) throws InterruptedException, BrokenBarrierException, TimeoutException：使当前线程等待，直到所有线程都到达屏障点，或者等待时间超过指定的时间。

boolean isBroken()：检查屏障是否已经被破坏。

void reset()：重置屏障到初始状态。

## 使用 Demo
### 示例 1：简单的CyclicBarrier使用
假设我们有一个任务，它需要分成多个部分并由多个线程并行执行，所有线程在完成各自部分后需要同步，然后再继续执行。

```plain
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

在这个示例中，三个线程并行执行任务的两个部分。在完成第一个部分后，它们会在屏障点等待，直到所有线程都完成第一个部分。然后，所有线程继续执行第二个部分。

### 示例 2：重复使用CyclicBarrier
CyclicBarrier可以被重复使用，这意味着在所有线程通过屏障点后，它可以被重置并再次使用。

```plain
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

在这个示例中，每个线程执行三个迭代的任务。在每个迭代中，它们会在屏障点等待，直到所有线程都完成当前迭代。然后，所有线程继续下一次迭代。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/zp1dv49hr71gretb>