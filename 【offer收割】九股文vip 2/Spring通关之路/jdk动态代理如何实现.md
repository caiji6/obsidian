# 👌jdk动态代理如何实现

# 口语化答案
好的，面试官。jdk 的动态代理主要是依赖Proxy 和InvocationHandler 接口。jdk 动态代理要求类必须有接口。在进行实现的时候，首先要定义接口，比如MyService，这个接口就是我们的正常功能的实现。但是希望在不更改MyService 的情况下增加，那么我们需要定义一个实现InvocationHandler 接口的实现类，同时在方法实现上面增加额外的逻辑。最后通过 Proxy 的 newProxyInstance 将二者结合到一起。就实现了动态代理。以上。

# 题目解析
大家不要觉得动态代理很难理解，按照这个步骤其实你发现很简单。记忆的过程和 cglib 对比着看，就很轻松，面试也是属于常考一点的题目。

# 面试得分点
InvocationHandler 增强，Proxy 创建代理

# 题目详细答案
JDK 动态代理主要依赖于java.lang.reflect.Proxy类和java.lang.reflect.InvocationHandler接口来实现。

## 实现步骤
**定义接口**：定义需要代理的接口。

**实现接口**：创建接口的实现类。

**创建调用处理器**：实现InvocationHandler接口，并在invoke方法中定义代理逻辑。

**创建代理对象**：通过Proxy.newProxyInstance方法创建代理对象。

## 代码 Demo
假设我们有一个简单的服务接口MyService和它的实现类MyServiceImpl，我们将通过 JDK 动态代理为MyService创建一个代理对象，并在方法调用前后添加日志。

#### 1. 定义接口
```plain
public interface MyService {
    void performTask();
}
```

#### 2. 实现接口
```plain
public class MyServiceImpl implements MyService {
    @Override
    public void performTask() {
        System.out.println("Performing task");
    }
}
```

#### 3. 创建调用处理器
```plain
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class LoggingInvocationHandler implements InvocationHandler {
    private final Object target;

    public LoggingInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Logging before method execution: " + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("Logging after method execution: " + method.getName());
        return result;
    }
}
```

#### 4. 创建代理对象并使用
```plain
import java.lang.reflect.Proxy;

public class MainApp {
    public static void main(String[] args) {
        // 创建目标对象
        MyService myService = new MyServiceImpl();

        // 创建调用处理器
        LoggingInvocationHandler handler = new LoggingInvocationHandler(myService);

        // 创建代理对象
        MyService proxyInstance = (MyService) Proxy.newProxyInstance(
                myService.getClass().getClassLoader(),
                myService.getClass().getInterfaces(),
                handler
        );

        // 调用代理对象的方法
        proxyInstance.performTask();
    }
}
```

## 详细解释
**1、 接口定义和实现**：

MyService是一个简单的接口，定义了一个方法performTask。

MyServiceImpl是MyService的实现类，实现了performTask方法。

**2、 调用处理器**：

LoggingInvocationHandler实现了InvocationHandler接口。它的invoke方法在代理对象的方法调用时被调用。invoke方法接收三个参数：proxy：代理对象。method：被调用的方法。args：方法参数。在invoke方法中，我们在方法调用前后添加了日志打印。

**3、 创建代理对象**：

使用Proxy.newProxyInstance方法创建代理对象。

newProxyInstance方法接收三个参数：类加载器：通常使用目标对象的类加载器。接口数组：目标对象实现的所有接口。调用处理器：实现了InvocationHandler接口的实例。

**4、 使用代理对象**：

通过代理对象调用方法时，实际调用的是LoggingInvocationHandler的invoke方法。

在invoke方法中，首先打印日志，然后通过反射调用目标对象的方法，最后再打印日志。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/st7cfy1f43xmusnd>