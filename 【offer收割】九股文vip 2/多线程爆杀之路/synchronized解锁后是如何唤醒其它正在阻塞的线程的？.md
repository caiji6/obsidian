# synchronized解锁后是如何唤醒其它正在阻塞的线程的？

# 题目详细答案
在 Java 中，当一个线程退出一个 synchronized 块或方法时，它会释放持有的锁。释放锁的操作会触发 JVM 去唤醒等待该锁的线程。具体的唤醒机制由 JVM 实现决定，但通常情况下，当锁被释放时，JVM 会选择一个或多个等待该锁的线程并将其唤醒，使其能够重新竞争锁的获取。

## 详细过程
### 锁的获取和释放：
当一个线程进入一个`synchronized`块或方法时，它会尝试获取该对象的监视器锁（monitor lock）。如果锁已经被其他线程持有，该线程将进入阻塞状态，等待锁被释放。当线程退出`synchronized`块或方法时，它会释放该监视器锁。

### 线程的等待和唤醒：
当一个线程在等待`synchronized`锁时，它会进入阻塞队列（也称为等待队列）。当锁被释放时，JVM 会从阻塞队列中选择一个或多个线程进行唤醒。被唤醒的线程会重新尝试获取锁。如果锁被其他线程抢先获取，唤醒的线程将再次进入阻塞状态，直到锁再次被释放。

## JVM 如何选择唤醒的线程
JVM 选择唤醒线程的具体策略可能因不同的 JVM 实现而异，但通常会遵循以下原则：

**公平性**：某些 JVM 实现可能会尝试实现某种程度的公平性，按照线程进入阻塞队列的顺序进行唤醒。

**非公平性**：其他实现可能采取非公平的策略，随机选择一个等待的线程进行唤醒。

```plain
public class SynchronizedExample {

    private static final Object lock=new Object();

    public static void main(String[] args) {
        Thread t1=new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread 1: Holding lock...");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Thread 1: Releasing lock...");
            }
        });

        Thread t2=new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread 2: Holding lock...");
            }
        });

        t1.start();
        try {
            Thread.sleep(100); // Ensure t1 starts first
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        t2.start();
    }
}
```

在这个示例中：

`Thread 1`首先获取锁并持有它一段时间。

`Thread 2`尝试获取锁但会被阻塞，直到`Thread 1`释放锁。

当`Thread 1`释放锁后，JVM 会唤醒`Thread 2`，使其能够获取锁并执行同步块中的代码。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fxvth92haqehpmgs>