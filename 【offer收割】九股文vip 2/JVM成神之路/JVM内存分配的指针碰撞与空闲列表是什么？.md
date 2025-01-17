# 👌JVM内存分配的指针碰撞与空闲列表是什么？

# 题目详细答案
指针碰撞（Bump-the-pointer）和空闲列表（Free-list）是两种常见的内存分配策略。

## 指针碰撞（Bump-the-pointer）
指针碰撞是一种高效的内存分配策略，适用于堆内存是连续且规整的情况。这种方法的基本思想是通过移动一个指针来分配内存。具体步骤如下：

1. **内存布局**：堆内存被划分为已使用的内存和空闲的内存，中间有一个指针（称为分配指针）作为分界线。
2. **分配内存**：当需要为新对象分配内存时，只需将分配指针向空闲内存方向移动一段与对象大小相等的距离。
3. **更新指针**：分配指针更新后，新的对象内存区域就被标记为已使用。

这种方法的优点是分配速度非常快，只需简单的指针移动操作。然而，它的缺点是在堆内存不规整（例如存在内存碎片）的情况下无法使用。



```plain
| 已使用内存 | 分配指针 | 空闲内存 |
|------------|----------|----------|
```

当分配一个对象时，分配指针向右移动：

```plain
| 已使用内存 | 已使用内存 | 分配指针 | 空闲内存 |
|------------|------------|----------|----------|
```

## 空闲列表（Free-list）
空闲列表是一种适用于堆内存不规整的情况下的内存分配策略。它通过维护一个列表来记录所有可用的空闲内存块。具体步骤如下：

1. **空闲列表**：JVM 维护一个空闲列表，记录所有可用的内存块及其大小。
2. **查找空闲块**：当需要为新对象分配内存时，JVM 会在空闲列表中查找一个足够大的内存块。
3. **分配内存**：找到合适的内存块后，将其从空闲列表中移除，并将其标记为已使用。如果内存块大于所需大小，可能会将剩余部分重新放回空闲列表中。
4. **回收内存**：当对象被垃圾回收器回收后，JVM 会将其内存块重新添加到空闲列表中。

这种方法的优点是能够更好地利用内存，适用于内存碎片较多的情况。然而，它的缺点是分配速度较慢，因为需要在空闲列表中查找合适的内存块。

#### 
假设空闲列表如下：

```plain
空闲列表: [块1(大小: 32), 块2(大小: 64), 块3(大小: 128)]
```

当需要分配一个大小为 50 的对象时，JVM 会在空闲列表中查找：

```plain
找到块2(大小: 64)
```

将块2分成两部分：

```plain
分配块2的前50个单位，剩余部分重新放回空闲列表
空闲列表: [块1(大小: 32), 块2剩余部分(大小: 14), 块3(大小: 128)]
```



**指针碰撞（Bump-the-pointer）**：适用于堆内存规整的情况，分配速度快，但不适用于内存碎片较多的情况。

**空闲列表（Free-list）**：适用于堆内存不规整的情况，能够更好地利用内存，但分配速度较慢。

这两种内存分配策略各有优缺点，JVM 会根据具体情况选择合适的策略，以优化内存分配和垃圾回收的效率。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/huc0ybzeaoxwbocc>