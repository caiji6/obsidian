# 👌HashMap的负载因子初始值为什么是0.75?

# 题目详细答案
HashMap的负载因子（load factor）初始值设为 0.75 是一个经过权衡的结果，主要考虑了性能和内存使用之间的平衡。

## 性能与内存使用的平衡
**查找性能**：在HashMap中，查找操作的时间复杂度接近 (O(1))。然而，当哈希表中的元素过多时，链地址法中的链表会变长，查找时间会增加。负载因子为 0.75 意味着在表达到 75% 满时进行扩容，这样可以保持链表的长度较短，从而保证查找操作的高效性。

**内存使用**：如果负载因子设置得太低（例如 0.5），HashMap会更频繁地扩容，需要更多的内存来存储未使用的桶。负载因子为 0.75 是一个较为合理的设置，可以在保证查找性能的同时，节约内存。

## 扩容频率
较高的负载因子（如 1.0）会减少扩容的频率，但会导致较长的链表或更多的哈希碰撞，从而影响查找性能。较低的负载因子（如 0.5）会增加扩容的频率，虽然可以减少碰撞，但会导致更多的空间浪费。

0.75 是一个折中的选择，它既能保证较少的哈希碰撞，又不会频繁地进行扩容，从而在性能和内存使用之间取得平衡。

## 实际应用中的经验
在实际应用中，0.75 被证明是一个有效的默认值。它在大多数情况下提供了良好的性能和较为合理的内存使用。尽管特定应用可能有不同的需求，但对于通用场景，这个默认值是经过大量实践验证的。

## 负载因子的灵活性
虽然 0.75 是默认值，开发者在创建HashMap时可以根据具体需求指定不同的负载因子。例如：

```plain
Map<Integer, String> map = newHashMap<>(initialCapacity, 0.5f);
```

在上述代码中，HashMap的负载因子被设置为 0.5，这可能适用于需要更高查找性能但内存使用不是主要考虑因素的场景。

HashMap默认负载因子为 0.75 是一个经过深思熟虑的选择，旨在平衡查找性能和内存使用。它在大多数情况下提供了良好的性能表现，同时避免了频繁扩容和过多的内存浪费。开发者可以根据具体需求调整负载因子，以适应不同的应用场景。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/nokyemihqmhdvppw>