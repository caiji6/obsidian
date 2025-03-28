# 👌ArrayList是如何扩容的？

# 题目详细答案
ArrayList的扩容机制是Java集合框架中一个重要的概念，它允许ArrayList在需要时自动增加其内部数组的大小以容纳更多的元素。主要有以下的步骤

1、初始容量和扩容因子：

当创建一个新的ArrayList对象时，它通常会分配一个初始容量，这个初始容量默认为10。

ArrayList的扩容因子是一个用于计算新容量的乘数，默认为1.5。

2、扩容触发条件：

当向ArrayList中添加一个新元素，并且该元素的数量超过当前数组的容量时，就会触发扩容操作。

3、扩容策略：

扩容时，首先根据当前容量和扩容因子计算出一个新的容量。新容量的计算公式为：

newCapacity = oldCapacity + (oldCapacity >> 1)，这实际上是将原容量增加50%（即乘以1.5）。

如果需要的容量大于Integer.MAX_VALUE - 8（因为数组的长度是一个int类型，其最大值是Integer.MAX_VALUE，但ArrayList需要预留一些空间用于内部操作），则会使用Integer.MAX_VALUE作为新的容量。

4、扩容过程：

创建一个新的数组，其长度为新计算的容量。

将原数组中的所有元素复制到新数组中。

将ArrayList的内部引用从原数组更新为新数组。

将新元素添加到新数组的末尾。

5、性能影响：

扩容过程涉及到内存分配和元素复制，可能会对性能产生一定的影响。因此，在使用ArrayList时，如果可能的话，最好预估需要存储的元素数量，并设置一个合适的初始容量，以减少扩容的次数。

6、扩容源码

![1719246984492-7f242ac2-c31e-4cdd-a074-bc448c5d3148.png](./img/a6dlsBfyxSbkioxG/1719246984492-7f242ac2-c31e-4cdd-a074-bc448c5d3148-695971.png)



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ghpqbh9ig3hu1coh>