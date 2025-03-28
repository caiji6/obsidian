# 👌HashMap怎么计算hashCode的？

# <font style="color:rgb(38, 38, 38);">题目详细答案</font>
HashMap使用键的hashCode()方法来生成哈希值，并对其进行一些处理，以提高哈希表的性能和均匀分布。

## 调用键的hashCode()方法
首先，HashMap调用键对象的hashCode()方法来获取一个整数哈希码。这个哈希码是由键对象的类定义的，通常是通过某种算法基于对象的内部状态计算出来的。

```plain
int hashCode= key.hashCode();
```

## 扰动函数 (Perturbation Function)
为了减少哈希冲突并使哈希码更加均匀地分布，HashMap对原始哈希码进行了一些额外的处理。这种处理被称为扰动函数。Java 8 及以后的HashMap实现使用以下算法来计算最终的哈希值：

```plain
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

这个算法的步骤如下：

1. **获取键的哈希码**：h = key.hashCode()
2. **右移 16 位**：h >>> 16
3. **异或运算**：h ^ (h >>> 16)

这种方法通过将高位和低位的哈希码混合在一起，减少了哈希冲突的概率，从而使得哈希码更加均匀地分布在哈希表的桶中。

## 计算数组索引
计算出扰动后的哈希值后，HashMap使用这个值来确定键值对在哈希表中的位置。通常，HashMap使用哈希值对数组的长度取模（取余数）来计算索引：

```plain
int index = (n - 1) & hash;
```

其中，n是哈希表数组的长度。n通常是 2 的幂，这样(n - 1)就是一个全 1 的二进制数，这使得按位与操作&可以有效地替代取模操作%，从而提高性能。

# demo
假设我们有一个键对象，其hashCode()返回值为 123456。那么，计算哈希值的过程如下：

1. 调用hashCode()方法：int hashCode = 123456;
2. 扰动函数计算：
    - h = 123456
    - h >>> 16 = 123456 >>> 16 = 1（右移 16 位）
    - hash = h ^ (h >>> 16) = 123456 ^ 1 = 123457
3. 计算数组索引（假设数组长度n为 16，即n - 1为 15）：
    - index = (15) & 123457 = 15 & 123457 = 1

最终，键值对将存储在哈希表数组的索引 1 位置。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/str1ewvagoc4qesr>