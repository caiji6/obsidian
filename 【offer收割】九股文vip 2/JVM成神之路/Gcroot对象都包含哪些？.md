# 👌Gc root对象都包含哪些？

# 题目详细答案
## 什么是GC Root
GC Root是垃圾回收器确定对象是否可达的起始点。在Java中，GC Root是一组特殊的对象，GC Root对象保证了这些对象及其引用链不会被垃圾回收器回收，因为它们是程序的起始点，其他对象通过它们间接可达，它确保了内存中的对象能够正确地被管理和清理，避免内存泄漏和无效引用的问题。

## gcroot 对象
**虚拟机栈（栈帧中的本地变量表）中的引用**：

每个线程都有一个虚拟机栈，栈帧中的本地变量表（Local Variable Table）包含了方法执行过程中用到的所有局部变量。这些局部变量可能包含对对象的引用。

**方法区中的类静态变量引用**：

方法区中存储了类的元数据，包括类的静态变量。这些静态变量可能引用对象。

**方法区中的常量引用**：

方法区还包含运行时常量池（Runtime Constant Pool），其中可能有对对象的引用。

**本地方法栈中的 JNI（Java Native Interface）引用**：

本地方法栈（Native Method Stack）用于本地方法的调用。本地方法可以通过 JNI 引用 Java 对象，这些引用也是 GC Roots。

**活动线程**：

所有正在运行的线程本身也是 GC Roots。

**类加载器**：

类加载器本身也是 GC Roots，因为它们负责加载类，而类加载器的引用链可以追溯到所有被加载的类及其静态变量。

**系统类**：

一些系统级的类，比如java.lang.Thread，java.lang.System等，也被视为 GC Roots。

**JNI 全局引用**：

JNI 中的全局引用（Global References）也是 GC Roots。

**JVM 内部的某些数据结构**：

JVM 内部的一些数据结构（如 JIT 编译器生成的代码中的引用）也可能被视为 GC Roots。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/sid6ac14ps3yg6pl>