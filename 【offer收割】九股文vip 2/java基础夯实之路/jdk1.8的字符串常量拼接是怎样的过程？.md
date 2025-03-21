# 👌jdk1.8的字符串常量拼接是怎样的过程？

# 题目详细答案
## 编译时优化
### 编译时常量折叠
对于编译时已知的字符串常量，Java 编译器会进行常量折叠（Constant Folding）。这意味着在编译阶段，编译器会直接计算出拼接结果，并将其作为一个单一的字符串常量存储在.class文件中。例如：在编译时，这段代码会被优化为：

```plain
String str = "Hello" + " " + "World";
```

```plain
String str = "Hello World";
```

### 非常量表达式：
如果拼接的字符串包含变量或方法调用，编译器不能在编译时确定结果，因此需要在运行时进行拼接。在 JDK 1.8 中，编译器会将这些拼接操作转换为使用StringBuilder的代码。例如：在编译时，这段代码会被转换为：

```plain
String str1 = "Hello";
String str2 = "World";
String result = str1 + " " + str2;
```

```plain
StringBuilder sb = new StringBuilder();
sb.append(str1);
sb.append(" ");
sb.append(str2);
String result = sb.toString();
```

## 运行时处理
### StringBuilder的使用
在运行时，对于非常量的字符串拼接，StringBuilder被用来构建最终的字符串。StringBuilder是可变的，因此可以高效地进行字符串的拼接操作。

例如：在运行时，StringBuilder会依次调用append方法，将各个部分拼接起来，并最终调用toString方法生成结果字符串。

```plain
String str1 = "Hello";
String str2 = "World";
String result = str1 + " " + str2;
```

### 性能优化
使用StringBuilder进行拼接比直接使用String的+操作符效率高得多，因为String是不可变的，每次拼接都会创建新的String对象，而StringBuilder则是可变的，可以在原有对象上进行修改。

### 示例
```plain
public class StringConcatExample {
    public static void main(String[] args) {
        // 编译时常量折叠
        String str1 = "Hello" + " " + "World"; // 直接变为 "Hello World"
        
        // 运行时拼接
        String str2 = "Hello";
        String str3 = "World";
        String result = str2 + " " + str3; // 使用 StringBuilder 进行拼接
    }
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/pdqz7ssg8kxkqdsh>