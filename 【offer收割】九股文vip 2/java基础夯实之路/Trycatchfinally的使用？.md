# 👌Try catch finally的使用？

# 题目详细答案
`try-catch-finally`是 Java 中用于异常处理的结构。它允许程序员捕获和处理在程序运行过程中可能发生的异常，并在执行完异常处理后执行一些清理工作。

```plain
try {
    // 可能抛出异常的代码
} catch (ExceptionType1 e1) {
    // 处理 ExceptionType1 类型的异常
} catch (ExceptionType2 e2) {
    // 处理 ExceptionType2 类型的异常
} finally {
    // 无论是否发生异常，都会执行的代码
}
```

**1、try 块**：包含可能抛出异常的代码。如果在`try`块中发生异常，控制权将立即转移到相应的`catch`块。

**2、catch 块**：用于捕获和处理特定类型的异常。可以有多个`catch`块来处理不同类型的异常。每个`catch`块都必须处理一种特定类型的异常。`catch`块的参数是异常类型和异常对象。

**3、finally 块**：包含确保执行的代码，无论是否发生异常。通常用于清理资源，如关闭文件、释放锁等。`finally`块是可选的，但如果存在，它会在`try`块和任何相关的`catch`块之后执行。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/iz0xcepes0f6xuk5>