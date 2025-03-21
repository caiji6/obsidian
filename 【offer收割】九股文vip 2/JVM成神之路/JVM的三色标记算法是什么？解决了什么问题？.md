# 👌JVM的三色标记算法是什么？解决了什么问题？

# 口语化回答
三色标记算法通过颜色标记和处理队列，解决了并发垃圾收集过程中对象引用变化导致的标记不准确问题。它确保所有存活对象都能被正确标记，从而避免了存活对象被错误回收。同时，写屏障技术的使用进一步保证了颜色不变性，确保算法在并发环境下的正确性。

# 题目详细答案
三色标记算法是一种用于垃圾回收的标记算法，通过将对象分为三种颜色（白色、灰色和黑色）来管理垃圾收集过程。它主要解决了在并发垃圾收集过程中如何正确标记存活对象的问题，避免了遗漏存活对象或错误标记对象的情况。

## 三色标记算法的基本概念
**白色**：表示对象尚未被垃圾收集器访问到。如果垃圾收集过程结束时对象仍然是白色，它将被视为不可达对象，随后被回收。

**灰色**：表示对象已被访问到，但其引用的对象尚未全部访问。灰色对象需要进一步扫描其引用的对象。

**黑色**：表示对象及其引用的对象都已被访问到。黑色对象不需要再扫描。

## 三色标记算法的工作流程
1. **初始化**：所有对象开始时都是白色的。
2. **标记根对象**：将根对象（GC Roots）标记为灰色，并将它们放入一个待处理队列。
3. **处理灰色对象**：

从队列中取出一个灰色对象，将其标记为黑色，将该对象引用的所有白色对象标记为灰色，并将这些灰色对象加入队列。

4. **重复步骤 3**，直到队列为空。
5. **清除白色对象**：所有剩余的白色对象都是不可达的，可以被回收。

## 解决的问题
三色标记算法主要解决了以下问题：

1. **并发标记的准确性**：在并发垃圾收集过程中，应用线程可能会修改对象引用，导致垃圾收集器标记不准确。三色标记算法通过颜色标记和处理队列，确保所有存活对象都能被正确标记。
2. **避免重复标记**：通过将对象分为三种颜色，垃圾收集器能有效避免重复标记对象，提高标记效率。
3. **处理对象引用变化**：在并发标记阶段，应用线程可能会增加或删除对象引用。三色标记算法通过维护颜色状态和处理队列，确保引用变化不会导致存活对象被错误回收。

## 颜色不变性
为了确保三色标记算法在并发环境下的正确性，通常需要维护以下两种不变性之一：

1. **强三色不变性**：黑色对象不能直接引用白色对象。这意味着如果一个黑色对象引用了一个白色对象，白色对象必须先被标记为灰色。
2. **弱三色不变性**：在标记阶段，白色对象只能通过灰色对象直接或间接引用。这意味着如果一个白色对象被引用，它一定会通过一个灰色对象。

## 写屏障
为了维护颜色不变性，垃圾收集器通常会使用写屏障技术。在对象引用发生变化时，写屏障会执行特定的操作，确保颜色不变性不被破坏。例如，在引用赋值操作时，写屏障可能会将目标对象标记为灰色，确保其不会被错误回收。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/cf83y4ktfuei8e8i>