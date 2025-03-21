# 👌为什么java多线程调用的是start方法不是run方法？

# 题目详细答案
## start()方法
start()方法的作用是启动一个新线程，并且使该线程进入就绪状态，等待操作系统的线程调度器来调度它执行。当你调用start()方法时，Java虚拟机会创建一个新的执行线程。在这个新的线程中，Java虚拟机会自动调用run()方法。调用start()方法后，原来的线程和新创建的线程可以并发执行。

## run()方法
run()方法包含了线程执行的代码，是你需要在新线程中执行的任务。如果直接调用run()方法，run()方法会在当前线程中执行，而不会启动一个新线程。直接调用run()方法不会创建新的线程，所有代码在调用run()方法的线程中顺序执行。

## 代码 Demo
```plain
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }

    public static void main(String[] args) {
        MyThread thread = new MyThread();

        // 调用 start() 方法
        thread.start(); // 这会启动一个新线程，run() 方法在新线程中执行

        // 调用 run() 方法
        thread.run(); // 这会在当前线程中执行 run() 方法，不会启动新线程
    }
}
```

## 为什么不能直接调用run()方法
1. **启动新线程**：start()方法负责启动一个新线程，而直接调用run()只是普通的方法调用，不会启动新线程。
2. **并发执行**：通过start()方法启动的新线程可以与原来的线程并发执行，而直接调用run()方法则是在当前线程中顺序执行。
3. **线程状态管理**：start()方法会使线程进入就绪状态，等待操作系统调度，而直接调用run()方法不会改变线程的状态管理。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/no8hu4h3uiq60392>