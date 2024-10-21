# 👌final有什么用？

# 题目详细答案
在Java中，final关键字可以用于类、方法和变量，分别有不同的作用。

## final变量
当一个变量被声明为final时，它的值在初始化之后就不能被改变。final变量必须在声明时或通过构造函数进行初始化。

```plain
public class Example {
    final int finalVar = 10; // 声明时初始化

    final int anotherFinalVar;

    public Example(int value) {
        anotherFinalVar = value; // 通过构造函数初始化
    }

    public void changeValue() {
        // finalVar = 20; // 编译错误，final变量的值不能被改变
    }
}
```

## final方法
当一个方法被声明为final时，它不能被子类重写（override）。这可以用来防止子类改变父类中关键的方法实现。

```plain
public class ParentClass {
    public final void finalMethod() {
        System.out.println("This is a final method.");
    }
}

public class ChildClass extends ParentClass {
    // @Override
    // public void finalMethod() { // 编译错误，无法重写final方法
    //     System.out.println("Trying to override final method.");
    // }
}
```

## final类
当一个类被声明为final时，它不能被继承。这对于创建不可变类（immutable class）或确保类的实现不被改变是非常有用的。

```plain
public final class FinalClass {
    // 类的内容
}

// public class SubClass extends FinalClass { // 编译错误，无法继承final类
//     // 子类内容
// }
```

## final和不可变对象
final关键字在创建不可变对象时非常有用。不可变对象的状态在创建之后不能被改变，这在多线程环境中尤其重要，因为它们是线程安全的。

```plain
public final class ImmutableClass {
    private final int value;

    public ImmutableClass(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

## final和局部变量
final也可以用于局部变量，尤其是在匿名类或lambda表达式中使用时。被声明为final的局部变量在方法执行期间不能被修改。

```plain
public class Example {
    public void method() {
        final int localVar = 10;

        // localVar = 20; // 编译错误，无法修改final局部变量

        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println(localVar); // 可以在匿名类中访问final局部变量
            }
        };

        runnable.run();
    }
}
```

## 总结
**final变量**：值不能被改变。

**final方法**：不能被重写。

**final类**：不能被继承。

**final局部变量**：在方法执行期间不能被修改，尤其在匿名类和lambda表达式中有用。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hwg0cw3oq56pfb35>