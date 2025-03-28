# 👌Java 线程池工作过程？

# 题目详细答案
1. 线程池刚创建时，里面没有一个线程。任务队列是作为参数传进来的。不过，就算队列里面有任务，线程池也不会马上执行它们。



2. 当调用 execute() 方法添加一个任务时，线程池会做如下判断：

a) 如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务；

b) 如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列；

c) 如果这时候队列满了，而且正在运行的线程数量小于 maximumPoolSize，那么还是要创建非核心线程立刻运行这个任务；

d) 如果队列满了，而且正在运行的线程数量大于或等于 maximumPoolSize，那么线程池会抛出异常 RejectExecutionException。



3. 当一个线程完成任务时，它会从队列中取下一个任务来执行。



4. 当一个线程无事可做，超过一定的时间（keepAliveTime）时，线程池会判断，如果当前运行的线程数大于 corePoolSize，那么这个线程就被停掉。所以线程池的所有任务完成后，它最终会收缩到 corePoolSize 的大小。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/vt2xmeun12eqnxt4>