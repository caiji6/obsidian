### 基本用法
#### 1. 创建ThreadLocal变量
```
public class ThreadLocalExample {
    private static ThreadLocal<Integer> threadLocal = new ThreadLocal<>();

    public static void main(String[] args) {
        threadLocal.set(42); // 设置当前线程的变量值
        System.out.println(threadLocal.get()); // 获取当前线程的变量值
        threadLocal.remove(); // 清除当前线程的变量值
    }
}
```
#### 2. 使用withInitial方法初始化
ThreadLocal提供了一个便捷的方法withInitial来设置初始值。
```
public class ThreadLocalExample {
    private static ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 1);

    public static void main(String[] args) {
        System.out.println(threadLocal.get()); // 输出: 1
        threadLocal.set(42);
        System.out.println(threadLocal.get()); // 输出: 42
        threadLocal.remove();
        System.out.println(threadLocal.get()); // 输出: 1
    }
}
```
### 多线程环境中的使用
ThreadLocal在多线程环境中非常有用，可以确保每个线程都有自己的独立变量副本。例如：
```
public class ThreadLocalExample {
    private static ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);

    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                int value = threadLocal.get();
                threadLocal.set(value + 1);
                System.out.println(Thread.currentThread().getName() + ": " + threadLocal.get());
            }
        };

        Thread thread1 = new Thread(task, "Thread-1");
        Thread thread2 = new Thread(task, "Thread-2");

        thread1.start();
        thread2.start();
    }
}
```
在这个示例中，ThreadLocal确保了Thread-1和Thread-2各自维护自己的计数器，而不会互相干扰。
### 典型应用场景
#### 1. 用户会话管理
在 Web 应用中，可以使用ThreadLocal存储每个用户的会话信息。
```
public class UserContext {
    private static ThreadLocal<String> userThreadLocal = ThreadLocal.withInitial(() -> null);

    public static void setCurrentUser(String user) {
        userThreadLocal.set(user);
    }

    public static String getCurrentUser() {
        return userThreadLocal.get();
    }

    public static void clear() {
        userThreadLocal.remove();
    }
}
```
#### 2. 数据库连接
在数据库操作中，可以使用ThreadLocal存储每个线程的数据库连接。
```
public class ConnectionManager {
    private static ThreadLocal<Connection> connectionThreadLocal = ThreadLocal.withInitial(() -> {
        // 创建并返回一个新的数据库连接
        return createNewConnection();
    });

    public static Connection getConnection() {
        return connectionThreadLocal.get();
    }

    public static void closeConnection() {
        Connection connection = connectionThreadLocal.get();
        if (connection != null) {
            connection.close();
            connectionThreadLocal.remove();
        }
    }

    private static Connection createNewConnection() {
        // 实现创建数据库连接的逻辑
    }
}
```
