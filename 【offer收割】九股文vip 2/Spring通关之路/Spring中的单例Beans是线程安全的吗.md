# 👌Spring中的单例Beans是线程安全的吗

# 口语化答案
好的，面试官。单例 bean 不是线程安全的。因为单例意味着在整个 spring 容器中会重复使用。一般来说 bean 里面不掺杂公用的变量，这种叫做无状态的 bean，在方法内部调整变量里面的数据。假设一个 bean 里面有一个 int 变量 10，bean 里面的方法都操作这个变量，那就会产生线程安全问题，解决的方式可以用锁啊，支持线程安全的类呀，这些方式。不过最好还是无状态的，这样不影响性能。以上

# 题目解析
非常常考的一道题，一方面考察大家对无状态的理解，一方面看大家平时写 spring 的时候是否合理。

# 面试得分点
无状态，不安全

# 题目详细答案
在 Spring 中，单例（Singleton）Beans 是默认的 Bean 范围（Scope）。这意味着 Spring 容器在整个应用程序生命周期内只会创建一个 Bean 实例，并在需要时重复使用该实例。

单例 Beans 本身并不是线程安全的。Spring 容器不会自动为你处理线程安全问题。如果你的单例 Bean 被多个线程同时访问，并且这些线程执行的操作会修改 Bean 的状态，那么你需要自己确保线程安全。

## Demo
```plain
public class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

在多线程环境中，如果多个线程同时调用increment()方法，可能会导致竞争条件，从而产生错误的结果。

## 解决方法
### 使用同步（synchronization）
可以在方法级别使用synchronized关键字，确保同一时间只有一个线程可以执行该方法：

```plain
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

### 使用java.util.concurrent包中的并发工具类
```plain
import java.util.concurrent.atomic.AtomicInteger;

public class Counter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```

### 使用无状态（Stateless）设计
如果可能，设计无状态的 Beans，这样就不需要担心线程安全问题。无状态的 Bean 不会保存任何会被多个线程共享的状态。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/okuyxvlt6fxlphrl>