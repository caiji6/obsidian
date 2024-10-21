# 👌Lock的实现方法

# 题目详细答案
## lock()
```plain
void lock();
```

这个方法用于获取锁。如果锁不可用，当前线程将被禁用以进行线程调度，并处于休眠状态，直到获得锁。可以通过以下几种方式实现：

**自旋锁**：线程不断轮询锁的状态，直到获取到锁。

**阻塞队列**：使用类似AbstractQueuedSynchronizer(AQS) 的机制，将线程加入到一个等待队列中，直到锁可用。

```plain
public class SimpleSpinLock implements Lock {
    private AtomicBoolean isLocked = new AtomicBoolean(false);

    @Override
    public void lock() {
        while (!isLocked.compareAndSet(false, true)) {
            // 自旋等待
        }
    }

    @Override
    public void unlock() {
        isLocked.set(false);
    }

    // 其他方法省略
}
```

## lockInterruptibly()
```plain
void lockInterruptibly() throws InterruptedException;
```

这个方法与lock()类似，但它允许线程在等待锁的过程中响应中断。实现时需要在等待过程中检查线程的中断状态。

```plain
public class InterruptibleSpinLock implements Lock {
    private AtomicBoolean isLocked = new AtomicBoolean(false);

    @Override
    public void lockInterruptibly() throws InterruptedException {
        while (!isLocked.compareAndSet(false, true)) {
            if (Thread.currentThread().isInterrupted()) {
                throw new InterruptedException();
            }
        }
    }

    @Override
    public void unlock() {
        isLocked.set(false);
    }

    // 其他方法省略
}
```

## tryLock()
```plain
boolean tryLock();
```

这个方法尝试获取锁，如果锁不可用，则立即返回false。可以使用非阻塞的方式实现。

```plain
public class TryLockSpinLock implements Lock {
    private AtomicBoolean isLocked = new AtomicBoolean(false);

    @Override
    public boolean tryLock() {
        return isLocked.compareAndSet(false, true);
    }

    @Override
    public void unlock() {
        isLocked.set(false);
    }

    // 其他方法省略
}
```

## tryLock(long time, TimeUnit unit)
```plain
boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
```

这个方法尝试在给定的时间内获取锁，如果超时则返回false。可以使用循环和时间检查来实现。

```plain
public class TimedSpinLock implements Lock {
    private AtomicBoolean isLocked = new AtomicBoolean(false);

    @Override
    public boolean tryLock(long time, TimeUnit unit) throws InterruptedException {
        long timeout = unit.toNanos(time);
        long startTime = System.nanoTime();
        while (!isLocked.compareAndSet(false, true)) {
            if (System.nanoTime() - startTime >= timeout) {
                return false;
            }
            if (Thread.currentThread().isInterrupted()) {
                throw new InterruptedException();
            }
        }
        return true;
    }

    @Override
    public void unlock() {
        isLocked.set(false);
    }

    // 其他方法省略
}
```

## unlock()
```plain
void unlock();
```

这个方法用于释放锁。通常通过更新内部状态来表示锁已释放。

```plain
public class SimpleSpinLock implements Lock {
    private AtomicBoolean isLocked = new AtomicBoolean(false);

    @Override
    public void unlock() {
        isLocked.set(false);
    }

    // 其他方法省略
}
```

## newCondition()
```plain
Condition newCondition();
```

这个方法用于返回与此Lock实例绑定的Condition实例。Condition实现需要与Lock的内部状态紧密结合，通常使用内部的等待队列来管理等待线程。

示例实现（简单版本）：

```plain
public class SimpleLockWithCondition implements Lock {
    private final AtomicBoolean isLocked = new AtomicBoolean(false);
    private final Queue<Thread> waitingThreads = new ConcurrentLinkedQueue<>();

    @Override
    public Condition newCondition() {
        return new ConditionObject();
    }

    private class ConditionObject implements Condition {
        @Override
        public void await() throws InterruptedException {
            Thread currentThread = Thread.currentThread();
            waitingThreads.add(currentThread);
            while (!waitingThreads.contains(currentThread)) {
                LockSupport.park(this);
            }
            if (Thread.interrupted()) {
                throw new InterruptedException();
            }
        }

        @Override
        public void signal() {
            Thread thread = waitingThreads.poll();
            if (thread != null) {
                LockSupport.unpark(thread);
            }
        }

        @Override
        public void signalAll() {
            for (Thread thread : waitingThreads) {
                LockSupport.unpark(thread);
            }
            waitingThreads.clear();
        }

        // 其他方法省略
    }

    @Override
    public void lock() {
        while (!isLocked.compareAndSet(false, true)) {
            // 自旋等待
        }
    }

    @Override
    public void unlock() {
        isLocked.set(false);
    }

    // 其他方法省略
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ho8wgyxc1bps5smo>