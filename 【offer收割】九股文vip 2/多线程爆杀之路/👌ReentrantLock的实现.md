ReentrantLock是 Java 中java.util.concurrent.locks包提供的一个可重入锁实现。它提供了比synchronized关键字更灵活的锁定机制。ReentrantLock允许同一个线程多次获取锁，并且在每次获取锁时需要相应地释放锁。
### 1. 基本原理
ReentrantLock的核心是基于AbstractQueuedSynchronizer(AQS) 实现的。AQS 是一个用于构建锁和其他同步器的框架，它使用一个 FIFO 队列来管理获取锁的线程。
#### AQS 的工作原理：

- **状态**：AQS 使用一个int类型的变量state来表示锁的状态。state的值为 0 表示锁未被占用，大于 0 表示锁被占用。
- **队列**：AQS 使用一个 FIFO 队列来管理被阻塞的线程。每个节点代表一个线程。
- **独占模式**：在独占模式下，只有一个线程可以持有锁。ReentrantLock就是使用这种模式。
### 2. ReentrantLock 的主要方法实现
#### 2.1lock()
```
public void lock() {
    sync.lock();
}
```
ReentrantLock通过内部类Sync来实现锁的具体逻辑。Sync继承自AbstractQueuedSynchronizer。
```
abstract static class Sync extends AbstractQueuedSynchronizer {
    abstract void lock();

    final boolean nonfairTryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
        if (c == 0) {
            if (compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(current);
                return true;
            }
        } else if (current == getExclusiveOwnerThread()) {
            int nextc = c + acquires;
            if (nextc < 0) // overflow
                throw new Error("Maximum lock count exceeded");
            setState(nextc);
            return true;
        }
        return false;
    }
}
```
具体的lock()方法在FairSync和NonfairSync两个子类中实现。NonfairSync是非公平锁实现，FairSync是公平锁实现。
#### 2.2unlock()
```
public void unlock() {
    sync.release(1);
}
```
unlock()方法通过调用 AQS 的release()方法来实现锁的释放。
```
public final boolean release(int arg) {
    if (tryRelease(arg)) {
        Node h = head;
        if (h != null && h.waitStatus != 0)
            unparkSuccessor(h);
        return true;
    }
    return false;
}

protected final boolean tryRelease(int releases) {
    int c = getState() - releases;
    if (Thread.currentThread() != getExclusiveOwnerThread())
        throw new IllegalMonitorStateException();
    boolean free = false;
    if (c == 0) {
        free = true;
        setExclusiveOwnerThread(null);
    }
    setState(c);
    return free;
}
```
在tryRelease()方法中，首先检查当前线程是否是持有锁的线程，然后减少state的值。如果state的值变为 0，表示锁被释放，并清除持有锁的线程信息。
#### 2.3tryLock()
```
public boolean tryLock() {
    return sync.nonfairTryAcquire(1);
}
```
tryLock()方法尝试获取锁，如果锁可用则立即返回true，否则返回false。
#### 2.4lockInterruptibly()
```
public void lockInterruptibly() throws InterruptedException {
    sync.acquireInterruptibly(1);
}
```
lockInterruptibly()方法在等待锁的过程中响应中断。
#### 2.5tryLock(long timeout, TimeUnit unit)
```
public boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException {
    return sync.tryAcquireNanos(1, unit.toNanos(timeout));
}
```
tryLock(long timeout, TimeUnit unit)方法在指定的时间内尝试获取锁，如果在超时时间内获取到锁则返回true，否则返回false。
### 3. 公平锁和非公平锁
ReentrantLock提供了公平锁和非公平锁两种实现。非公平锁在竞争激烈的情况下可能会导致某些线程长时间得不到锁，而公平锁则会按照线程请求锁的顺序来分配锁。
#### 非公平锁
非公平锁在创建ReentrantLock时使用默认构造函数：
```
public ReentrantLock() {
    sync = newNonfairSync();
}
```
#### 公平锁
公平锁在创建ReentrantLock时通过构造函数指定：
```
public ReentrantLock(boolean fair) {
    sync = fair ? newFairSync() : newNonfairSync();
}
```
