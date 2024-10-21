# 👌short s1 = 1; s1 = s1 + 1;有错吗?short s1 = 1; s1 += 1;有错吗？

# 题目详细答案
## 代码 1
```plain
short s1 = 1;
s1 = s1 + 1;
```

在这个代码片段中，s1 = s1 + 1会导致编译错误。s1是一个short类型的变量。1是一个int类型的常量。在表达式s1 + 1中，s1会被提升为int类型，然后进行加法运算，结果是一个int类型的值。尝试将int类型的结果赋值给short类型的变量s1时，编译器会报错，因为这是一个潜在的精度丢失，需要显式的类型转换。

```plain
Type mismatch: cannot convert from int to short
```

要修复这个错误，可以进行显式的类型转换：

```plain
short s1 = 1;
s1 = (short) (s1 + 1);
```

## 代码 2
```plain
short s1 = 1;
s1 += 1;
```

这个代码片段是正确的，不会导致编译错误。原因如下：s1 += 1是一个复合赋值运算符，它等价于s1 = (short)(s1 + 1)。在复合赋值运算符中，隐式地进行了类型转换，所以不会出现类型不匹配的问题。

## 总结
short s1 = 1; s1 = s1 + 1;会导致编译错误，因为s1 + 1的结果是int类型，不能直接赋值给short类型的变量。

short s1 = 1; s1 += 1;是正确的，因为复合赋值运算符+=会隐式地进行类型转换。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ci99e49w2lf2so02>