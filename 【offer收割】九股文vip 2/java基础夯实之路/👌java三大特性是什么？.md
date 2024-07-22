Java 编程语言的三大特性是面向对象编程（OOP）的核心概念：封装、继承和多态。这些特性使得 Java 程序具有良好的结构和可维护性。
### 1. 封装（Encapsulation）
封装是将对象的状态（属性）和行为（方法）组合在一起，并对外隐藏对象的内部细节，只暴露必要的接口。通过封装，可以保护对象的状态不被外部直接修改，增强了代码的安全性和可维护性。

- **实现方式**：
   - 使用private关键字将属性声明为私有的。
   - 提供public的 getter 和 setter 方法来访问和修改私有属性。

**示例**：
```
public class Person {
    private String name;
    private int age;

    // Getter 方法
    public String getName() {
        return name;
    }

    // Setter 方法
    public void setName(String name) {
        this.name = name;
    }

    // Getter 方法
    public int getAge() {
        return age;
    }

    // Setter 方法
    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        }
    }
}
```
### 2. 继承（Inheritance）
继承是面向对象编程中的一个机制，通过继承，一个类可以继承另一个类的属性和方法，从而实现代码的重用。被继承的类称为父类（超类），继承的类称为子类（派生类）。

- **实现方式**：
   - 使用extends关键字来声明一个类继承另一个类。

**示例**：
```
public class Animal {
    public void eat() {
        System.out.println("This animal eats food.");
    }
}

public class Dog extends Animal {
    public void bark() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.eat();  // 调用继承自 Animal 类的方法
        myDog.bark(); // 调用 Dog 类的方法
    }
}
```
### 3. 多态（Polymorphism）
多态是指同一个方法在不同对象中具有不同的实现方式。多态性允许对象在不同的上下文中以不同的形式表现。多态可以通过方法重载（Overloading）和方法重写（Overriding）来实现。

- **方法重载**：在同一个类中，方法名相同但参数列表不同。
- **方法重写**：在子类中重新定义父类中的方法。

**示例**：
```
// 方法重载
public class MathUtils {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}

// 方法重写
public class Animal {
    public void makeSound() {
        System.out.println("Some generic animal sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Animal();
        Animal myDog = new Dog();

        myAnimal.makeSound(); // 输出: Some generic animal sound
        myDog.makeSound();    // 输出: Bark
    }
}
```
### 总结

- **封装**：通过将数据和方法封装在类中，并使用访问控制符来保护数据。
- **继承**：通过继承机制，实现代码的重用和扩展。
- **多态**：通过方法重载和方法重写，实现同一方法在不同对象中的不同表现。
