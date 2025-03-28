# 👌switch是否能作用在byte上，是否能作用在long上，是否能作用在String上?

# 题目详细答案
在Java中，switch语句是一种多分支选择结构，用于根据变量的值选择执行不同的代码块。switch语句可以作用于以下几种数据类型：

1. **byte、short、char、int**：这些都是基本数据类型，switch语句可以直接作用于它们。
2. **枚举类型（enum）**：从Java 5开始，switch语句可以作用于枚举类型。
3. **字符串类型（String）**：从Java 7开始，switch语句可以作用于String对象。
4. **包装类型（Wrapper Classes）**：从Java 5开始，switch语句可以作用于Byte、Short、Character、Integer等包装类。

## switch能作用于byte上吗？
switch语句可以作用于byte类型。

```plain
byte b = 1;
switch (b) {
    case 1:
        System.out.println("Byte value is 1");
        break;
    case 2:
        System.out.println("Byte value is 2");
        break;
    default:
        System.out.println("Byte value is neither 1 nor 2");
}
```

## switch能作用于long上吗？
不可以，switch语句不能直接作用于long类型。如果需要对long类型的变量进行类似switch的处理，可以使用if-else语句。

```plain
long l = 10L;
// 下面的代码是非法的
/*
switch (l) {
    case 10L:
        System.out.println("Long value is 10");
        break;
    default:
        System.out.println("Long value is not 10");
}
*/
```

## switch能作用于String上吗？
从Java 7开始，switch语句可以作用于String类型。

```plain
String str = "hello";
switch (str) {
    case "hello":
        System.out.println("String is hello");
        break;
    case "world":
        System.out.println("String is world");
        break;
    default:
        System.out.println("String is neither hello nor world");
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/loe3d1clng74sm69>