# 👌动态代理导致的频繁full gc

[此处为语雀卡片，点击链接查看](https://www.yuque.com/jingdianjichi/xyxdsi/brodotmb4ub1u3co#azw3T)

这道题也是一样的，当你简历写了 jvm 调优。还是一样，jvm 调优案例！

本案例为：jvm动态代理导致的频繁full gc！

本性能优化案例是通用的！大家消化完成，直接套到自己项目即可！

<font style="color:#DF2A3F;">本题配备了视频助于理解。</font>

# <font style="color:rgb(34, 34, 34);">背景</font>
在我们 618 的时候，需要对系统做压测，刚开始10并发进行压测，cpu压到了100%但是系统最大qps才200多。通过JVM监控查看JVM younggc很频繁，fullGC数量为零。

## **<font style="color:rgb(34, 34, 34);">排查过程</font>**
cpu 达到100% 则先看cpu使用率最高是哪个进程，可以直接通过linux命令 top查看，找到对应的进程ID，发现正是压测的java系统进程ID，找到进程ID后，然后在查找该进程下CPU使用率最高是哪个线程，可以通过top -p 进程ID -H 命令显示线程使用cpu信息，拿到一个pid。

PID列则为十进制显示的线程ID，然后转换为16进制通过jstack 系统进程ID | grep 15e 进制线程ID 可以找到对应的线程信息如下，也就是该线程占用了一半左右的cpu。

```plain
jstack 222 | grep 15e
```

执行完命令之后，可以看到一个 Finalizer 线程。

Finalizer线程是个单一职责的线程。这个线程会不停的循环等待java.lang.ref.Finalizer.ReferenceQueue中的新增对象。一旦Finalizer线程发现队列中出现了新的对象，它会弹出该对象，调用它的finalize()方法，将该引用从Finalizer类中移除，因此下次GC再执行的时候，这个Finalizer实例以及它引用的那个对象就可以回垃圾回收掉了。

说明Finalizer的队列中有许多的等待回收的垃圾对象，可以通过命令查看等待回收的对象都有哪些。

```plain
jmap -finalizerinfo 进程ID
```

执行命令后显示结果如下

<font style="color:rgb(51, 51, 51);">Count Class description</font>

<font style="color:rgb(51, 51, 51);">-------------------------------------------------------</font>

<font style="color:rgb(51, 51, 51);">32221 com.XXXXXXX$$EnhancerByCGLIB$$200e6ee6</font>

<font style="color:rgb(51, 51, 51);">14908 com.XXXXXXXX$$EnhancerByCGLIB$$a59933de</font>

<font style="color:rgb(51, 51, 51);">11982 java.util.zip.Deflater</font>

<font style="color:rgb(51, 51, 51);">1 java.net.SocksSocketImpl</font>

发现有好多的自定义对象，通过类名可以看到这些对象都是通过CGLIB动态代理创建的，而这些动态代理类都默认实现了finalize方法，导致这些对象在进行垃圾回收时必须先要执行finalize方法，所以都积压到了finalizer的队列中。

## **<font style="color:rgb(34, 34, 34);">解决方案</font>**
选择复用对象，不要每次都new一个 cglib 代理的对象。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/brodotmb4ub1u3co>