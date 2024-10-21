# 👌instanceof关键字的作用?

# 题目详细答案
instanceof关键字在Java中用于测试一个对象是否是一个特定类的实例，或者是该类的子类或实现类的实例。

它是一个二元操作符，返回一个布尔值（true或false）。

```plain
object instanceof ClassName
```

object：要进行类型检查的对象。

ClassName：要检查的类或接口。

## 主要作用
**类型检查**：确定对象是否是某个类的实例或是该类的子类的实例。

**避免类型转换异常**：在进行类型转换之前，使用instanceof可以防止ClassCastException异常。

## 代码 Demo
```plain
public class InstanceofExample {
    public static void main(String[] args) {
        Animal animal = new Dog();
        Dog dog = new Dog();
        Cat cat = new Cat();

        // 类型检查
        System.out.println(animal instanceof Animal); // true
        System.out.println(animal instanceof Dog);    // true
        System.out.println(animal instanceof Cat);    // false

        // 避免类型转换异常
        if (animal instanceof Dog) {
            Dog d = (Dog) animal;
            System.out.println("Animal is a Dog");
        }

        if (animal instanceof Cat) {
            Cat c = (Cat) animal; // 这段代码不会执行，因为animal并不是Cat的实例
        } else {
            System.out.println("Animal is not a Cat");
        }
    }
}

class Animal {}
class Dog extends Animal {}
class Cat extends Animal {}
```

## 注意
**空值检查**：如果左操作数为null，instanceof会返回false，因为null不是任何类的实例。

**编译时检查**：instanceof会在编译时进行类型检查，如果类型之间没有关系（例如不在同一个继承树中），编译器会报错。

### 代码 Demo（空值检查）
```plain
public class NullCheckExample {
    public static void main(String[] args) {
        Animal animal = null;
        System.out.println(animal instanceof Animal); // false
    }
}
```

### 代码 Demo（编译时检查）
```plain
public class CompileCheckExample {
    public static void main(String[] args) {
        String str = "hello";
        // 编译错误，因为String和Integer没有关系
        // System.out.println(str instanceof Integer); 
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/qv5rimy3vagwlgn2>