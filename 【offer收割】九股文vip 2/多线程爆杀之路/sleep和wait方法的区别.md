# 👌sleep和wait方法的区别

# 题目详细答案
Thread.sleep()和Object.wait()是Java中用于控制线程行为的两种方法，但它们有着显著的区别。

## 区别
| | sleep 方法 | object.wait 方法 |
| --- | --- | --- |
| 定义 | Thread.sleep(long millis)是一个静态方法，属于Thread类。 | wait()是一个实例方法，属于Object类。 |
| 作用 | 使当前线程进入休眠状态，暂停执行一段时间（以毫秒为单位）。 | 使当前线程等待，直到另一个线程调用该对象的notify()或notifyAll()方法来唤醒它。 |
| 锁状态 | 调用sleep方法时，线程不会释放它所持有的任何锁。 | 调用wait方法时，线程必须持有该对象的监视器锁，并且会释放该锁，进入等待状态。 |
| 唤醒 | 在指定的时间到期后，线程会自动从休眠状态中醒来并继续执行，不需要其他线程显式唤醒它。 | 线程必须被其他线程显式唤醒，调用notify()或notifyAll()方法，或者在超时等待的情况下，时间到期后自动唤醒。 |
| 异常 | sleep方法会抛出InterruptedException，如果线程在休眠期间被中断。 | wait方法会抛出InterruptedException，如果线程在等待期间被中断。 |
| 场景 | 通常用于暂停线程的执行，模拟延迟或让出CPU时间片。 | 通常用于线程间通信，协调线程之间的执行顺序。 |




## Thread.sleep()示例
```plain
public class SleepExample {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            try {
                System.out.println("Thread is going to sleep");
                Thread.sleep(2000); // 休眠2秒
                System.out.println("Thread woke up");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        thread.start();
    }
}
```

## Object.wait()示例
```plain
public class WaitNotifyExample {
    private static final Object lock = new Object();

    public static void main(String[] args) {
        Thread waitingThread = new Thread(() -> {
            synchronized (lock) {
                try {
                    System.out.println("Thread is waiting");
                    lock.wait(); // 进入等待状态，并释放锁
                    System.out.println("Thread is resumed");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread notifyingThread = new Thread(() -> {
            synchronized (lock) {
                try {
                    Thread.sleep(2000); // 休眠2秒
                    System.out.println("Thread is going to notify");
                    lock.notify(); // 唤醒等待线程
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        waitingThread.start();
        notifyingThread.start();
    }
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/uvxwdbep33g1bbmu>