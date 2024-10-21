# ConcurrentHashMap 为什么 key 和 value 不能为 null?为什么HashMap可以存？

`ConcurrentHashMap`不允许`key`和`value`为`null`，而`HashMap`可以，这是因为它们的设计目标和使用场景不同。

## ConcurrentHashMap
**线程安全性**：`ConcurrentHashMap`设计用于并发环境，允许多个线程同时访问。如果允许`null`值，无法区分`get(key)`返回`null`是因为键不存在还是键的值为`null`，这会导致歧义。

**避免潜在的错误**：禁止`null`值可以避免一些潜在的编程错误，例如误判键的存在性。通过抛出`NullPointerException`，可以强制开发者在使用时处理`null`值，提升代码的健壮性。

### HashMap
**灵活性**：`HashMap`设计用于非并发环境，允许更大的灵活性。允许`null`作为键或值，方便处理某些特殊场景，如表示缺失值。

**简单实现**：在单线程环境中，`HashMap`的操作不需要考虑并发问题，因此可以直接使用`null`来表示键或值的缺失。

`ConcurrentHashMap`的设计目标是高效地支持并发访问，因此限制`null`以避免歧义和潜在错误。

`HashMap`则更关注灵活性和易用性，在单线程环境下允许`null`来提供更多的使用场景。

这种设计差异反映了两者在使用场景和目标上的不同侧重。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/dwmf5k52u7zp93lf>