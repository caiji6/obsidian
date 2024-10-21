# 👌什么是 AQS（抽象的队列同步器）

# 题目详细答案
AQS（Abstract Queued Synchronizer，抽象的队列同步器）是Java并发包（java.util.concurrent）中的一个框架，用于构建锁和其他同步器（如信号量、读写锁等）。AQS通过一个FIFO（先进先出）的等待队列来管理线程的排队和唤醒，简化了同步器的实现。

## AQS的核心概念
**状态（state）**：AQS通过一个整型变量state来表示同步状态。不同的同步器可以根据自己的需求定义state的含义，例如对于独占锁，state可以表示锁的持有状态；对于共享锁，state可以表示可用资源的数量。

**独占模式（Exclusive Mode）**：独占模式下，只有一个线程能获取同步状态，其他线程必须等待。例如，ReentrantLock就是基于独占模式实现的。

**共享模式（Shared Mode）**：共享模式下，多个线程可以同时获取同步状态。例如，Semaphore和CountDownLatch就是基于共享模式实现的。

**等待队列（Wait Queue）**：AQS内部维护一个FIFO等待队列，用于管理被阻塞的线程。当线程获取同步状态失败时，会被加入到等待队列中，等待其他线程释放同步状态后被唤醒。

## AQS的工作原理
AQS通过以下几个核心方法来实现同步器的功能：

1. **acquire(int arg)**：以独占模式获取同步状态，如果获取失败，则将当前线程加入等待队列，并阻塞直到同步状态可用。
2. **release(int arg)**：以独占模式释放同步状态，唤醒等待队列中的下一个线程（如果有）。
3. **acquireShared(int arg)**：以共享模式获取同步状态，如果获取失败，则将当前线程加入等待队列，并阻塞直到同步状态可用。
4. **releaseShared(int arg)**：以共享模式释放同步状态，唤醒等待队列中的所有线程（如果有）。
5. **tryAcquire(int arg)**：尝试以独占模式获取同步状态，返回true表示获取成功，返回false表示获取失败。需要由具体的同步器实现。
6. **tryRelease(int arg)**：尝试以独占模式释放同步状态，返回true表示释放成功，返回false表示释放失败。需要由具体的同步器实现。
7. **tryAcquireShared(int arg)**：尝试以共享模式获取同步状态，返回大于等于0的值表示获取成功，返回负值表示获取失败。需要由具体的同步器实现。
8. **tryReleaseShared(int arg)**：尝试以共享模式释放同步状态，返回true表示释放成功，返回false表示释放失败。需要由具体的同步器实现。

## AQS实现简单独占锁
```plain
import java.util.concurrent.locks.AbstractQueuedSynchronizer;

public class SimpleLock {
    private final Sync sync = new Sync();

    private static class Sync extends AbstractQueuedSynchronizer {
        @Override
        protected boolean tryAcquire(int arg) {
            if (compareAndSetState(0, 1)) {
                setExclusiveOwnerThread(Thread.currentThread());
                return true;
            }
            return false;
        }

        @Override
        protected boolean tryRelease(int arg) {
            if (getState() == 0) {
                throw new IllegalMonitorStateException();
            }
            setExclusiveOwnerThread(null);
            setState(0);
            return true;
        }

        @Override
        protected boolean isHeldExclusively() {
            return getState() == 1;
        }
    }

    public void lock() {
        sync.acquire(1);
    }

    public void unlock() {
        sync.release(1);
    }

    public boolean isLocked() {
        return sync.isHeldExclusively();
    }
}
```

+ tryAcquire(int arg)方法尝试获取锁，通过CAS操作将state从0设置为1。
+ tryRelease(int arg)方法释放锁，通过将state设置为0并清除当前线程的持有状态。
+ lock()方法通过调用acquire(1)获取锁。
+ unlock()方法通过调用release(1)释放锁。
+ isLocked()方法检查当前锁是否被持有。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hkpgnynp5gwpf9ys>