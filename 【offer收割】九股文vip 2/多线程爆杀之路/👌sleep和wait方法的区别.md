Thread.sleep()和Object.wait()是Java中用于控制线程行为的两种方法，但它们有着显著的区别。
### Thread.sleep()方法

1. **定义**：Thread.sleep(long millis)是一个静态方法，属于Thread类。
2. **作用**：使当前线程进入休眠状态，暂停执行一段时间（以毫秒为单位）。
3. **锁状态**：调用sleep方法时，线程不会释放它所持有的任何锁。
4. **唤醒**：在指定的时间到期后，线程会自动从休眠状态中醒来并继续执行，不需要其他线程显式唤醒它。
5. **异常**：sleep方法会抛出InterruptedException，如果线程在休眠期间被中断。
6. **使用场景**：通常用于暂停线程的执行，模拟延迟或让出CPU时间片。
### Object.wait()方法

1. **定义**：wait()是一个实例方法，属于Object类。
2. **作用**：使当前线程等待，直到另一个线程调用该对象的notify()或notifyAll()方法来唤醒它。
3. **锁状态**：调用wait方法时，线程必须持有该对象的监视器锁，并且会释放该锁，进入等待状态。
4. **唤醒**：线程必须被其他线程显式唤醒，调用notify()或notifyAll()方法，或者在超时等待的情况下，时间到期后自动唤醒。
5. **异常**：wait方法会抛出InterruptedException，如果线程在等待期间被中断。
6. **使用场景**：通常用于线程间通信，协调线程之间的执行顺序。
### 代码示例
#### Thread.sleep()示例
```
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
#### Object.wait()示例
```
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
### 总结

- **Thread.sleep()**：
   - 使当前线程暂停执行一段时间。
   - 不释放任何锁。
   - 在时间到期后自动恢复执行。
- **Object.wait()**：
   - 使当前线程等待，直到被其他线程显式唤醒或时间到期。
   - 释放当前对象的监视器锁。
   - 需在synchronized块或方法内调用。
