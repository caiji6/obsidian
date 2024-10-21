# 👌JVM的类命名空间如何理解？

# 题目详细答案
JVM 的类命名空间是指 JVM 在运行时用来区分和管理不同类加载器加载的类的机制。

## 类命名空间的基本概念
在 JVM 中，每个类加载器都有自己的命名空间。一个类的完全限定名（即类的全路径名，例如com.example.MyClass）在 JVM 的命名空间中是唯一的，但同一个完全限定名的类可以由不同的类加载器加载，从而在不同的命名空间中存在多个版本。

## 类命名空间的工作原理
1. **双亲委派模型**：类加载器在加载一个类时，首先会将请求委派给父类加载器。如果父类加载器无法找到该类，才会由当前类加载器尝试加载。这种机制确保了核心类库的优先加载和安全性。
2. **类的唯一性**：在 JVM 中，一个类由其完全限定名和加载它的类加载器共同决定。即使两个类的完全限定名相同，但如果它们是由不同的类加载器加载的，那么它们在 JVM 中被认为是不同的类。

## 类命名空间示例
```plain
import java.lang.reflect.Method;

public class ClassLoaderNamespaceDemo {
    public static void main(String[] args) throws Exception {
        // 创建两个自定义类加载器
        ClassLoader classLoader1 = new CustomClassLoader();
        ClassLoader classLoader2 = new CustomClassLoader();

        // 使用不同的类加载器加载同一个类
        Class<?> class1 = classLoader1.loadClass("com.example.MyClass");
        Class<?> class2 = classLoader2.loadClass("com.example.MyClass");

        // 比较两个类对象是否相同
        System.out.println(class1 == class2); // 输出 false，说明类对象在不同的命名空间中

        // 实例化对象并调用方法
        Object obj1 = class1.getDeclaredConstructor().newInstance();
        Object obj2 = class2.getDeclaredConstructor().newInstance();

        Method method1 = class1.getMethod("sayHello");
        Method method2 = class2.getMethod("sayHello");

        method1.invoke(obj1);
        method2.invoke(obj2);
    }
}

// 自定义类加载器
class CustomClassLoader extends ClassLoader {
    @Override
    public Class<?> loadClass(String name) throws ClassNotFoundException {
        if (name.startsWith("com.example")) {
            // 自定义加载逻辑，例如从文件系统或网络加载类字节码
            byte[] classData = getClassData(name);
            if (classData != null) {
                return defineClass(name, classData, 0, classData.length);
            }
        }
        return super.loadClass(name);
    }

    private byte[] getClassData(String className) {
        // 实现类加载的逻辑，例如从文件系统或网络加载类字节码
        return null;
    }
}
```

CustomClassLoader是一个自定义类加载器。我们创建了两个CustomClassLoader实例，并分别使用它们加载同一个类com.example.MyClass。由于这两个类加载器是不同的，因此它们各自的命名空间也是不同的，即使类的完全限定名相同，加载后的类对象也是不同的。

## 类命名空间的应用
1. **模块化**：通过使用不同的类加载器，可以实现模块化的类加载，每个模块有自己的命名空间，互不干扰。
2. **隔离**：在应用服务器（如 Tomcat）中，不同的应用程序使用不同的类加载器，从而实现类的隔离，避免类冲突。
3. **插件系统**：在插件系统中，每个插件可以使用自己的类加载器加载类，确保插件之间的独立性和隔离性。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fenszfhd3vruroif>