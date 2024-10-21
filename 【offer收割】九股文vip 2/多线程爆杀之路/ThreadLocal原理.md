# 👌ThreadLocal原理

# 题目详细答案
ThreadLocal的实现依赖于每个线程内部维护的一个ThreadLocalMap对象。每个线程都有自己的ThreadLocalMap，而ThreadLocalMap中存储了所有ThreadLocal变量及其对应的值。

## 主要组成部分
1. **ThreadLocal 类**：提供了set()、get()、remove()等方法，用于操作线程局部变量。
2. **ThreadLocalMap 类**：是ThreadLocal的内部静态类，用于存储ThreadLocal变量及其值。
3. **Thread 类**：每个线程内部都有一个ThreadLocalMap实例。

## 工作机制
1. **创建****ThreadLocal****变量**：当创建一个ThreadLocal变量时，实际上并没有分配存储空间。
2. **获取值 (****get()****方法)**：当调用get()方法时，当前线程会通过自己的ThreadLocalMap获取ThreadLocal变量的值。如果不存在，则调用initialValue()方法获取初始值。
3. **设置值 (****set()****方法)**：当调用set()方法时，当前线程会通过自己的ThreadLocalMap设置ThreadLocal变量的值。
4. **删除值 (****remove()****方法)**：当调用remove()方法时，当前线程会通过自己的ThreadLocalMap删除ThreadLocal变量的值。

## 代码 Demo
```plain
public class ThreadLocal<T> {
    // 获取当前线程的 ThreadLocalMap
    private T getMap(Thread t) {
        return t.threadLocals;
    }

    // 获取当前线程的 ThreadLocalMap 中的值
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }

    // 设置当前线程的 ThreadLocalMap 中的值
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }

    // 删除当前线程的 ThreadLocalMap 中的值
    public void remove() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.remove(this);
    }

    // 初始化值
    protected T initialValue() {
        return null;
    }

    // 设置初始值
    private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
        return value;
    }

    // 创建 ThreadLocalMap
    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }

    // ThreadLocalMap 类
    static class ThreadLocalMap {
        static class Entry extends WeakReference<ThreadLocal<?>> {
            Object value;
            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }

        private Entry[] table;

        ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
            table = new Entry[INITIAL_CAPACITY];
            int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
            table[i] = new Entry(firstKey, firstValue);
            size = 1;
            setThreshold(INITIAL_CAPACITY);
        }

        // 其他方法省略
    }
}
```

## 使用场景
1. **数据库连接**：在多线程环境中，每个线程可以使用自己的数据库连接，避免连接共享带来的问题。
2. **用户会话**：在 Web 应用中，每个线程可以维护自己的用户会话信息。
3. **事务管理**：在分布式系统中，每个线程可以维护自己的事务上下文，确保事务的一致性。
4. **线程安全的工具类**：例如SimpleDateFormat，它不是线程安全的，可以使用ThreadLocal让每个线程都有自己的实例。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/vmig344fhaluugtt>