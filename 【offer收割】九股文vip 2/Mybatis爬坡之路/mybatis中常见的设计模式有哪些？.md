# 👌mybatis中常见的设计模式有哪些？

# 口语化回答
好的，面试官。mybatis 中应用了大量的设计模式，常见的有工厂模式，单例模式，代理模式等。比如工厂模式，mybatis 的核心 sqlsessionfactory，就是采用了工厂模式，帮助我们来创建 sqlsession 的实例。又比如，mybatis 的所有配置，其实是由一个Configuration 类，来进行管理的，这个类在整个生命周期中，仅此一份，也就是应用了单例模式。再比如用 mybatis 的时候，非常常用的 mapper 接口。通过调用 mapper 接口，就可以执行与之对应的 xml 的 sql 语句，这个原理其实是使用了代理模式，帮助我们将 mapper 代理出一个代理实现类，来增加一个功能。以上。

# 题目解析
高频题，这个其实问的非常多，大家在下面列出来的这些东西里面。选择三种设计模式就可以了。

# 面试得分点
工厂，单例，代理，模版方法，建造者，装饰器，策略，责任链

# 题目详细答案
## 工厂模式
在创建复杂对象时使用。MyBatis 使用SqlSessionFactory来创建SqlSession实例。SqlSessionFactory本身是由SqlSessionFactoryBuilder创建的。

```plain
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

## 单例模式
MyBatis 的Configuration类使用了单例模式，确保在一个SqlSessionFactory中只有一个Configuration实例。

```plain
Configuration configuration = sqlSessionFactory.getConfiguration();
```

## 代理模式
为其他对象提供一种代理以控制对这个对象的访问。MyBatis 使用动态代理为 Mapper 接口生成实现类，从而在调用 Mapper 方法时执行 SQL 语句。

```plain
UserMapper mapper = session.getMapper(UserMapper.class);
User user = mapper.selectUserById(1);
```

## 模板方法模式
定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。MyBatis 的Executor接口及其实现类使用了模板方法模式，定义了执行 SQL 语句的基本流程，而具体的执行细节由子类实现。

```plain
public class BaseExecutor implements Executor {
    public <E> List<E> query(...) {
        // 模板方法，定义查询的基本流程
        // 具体查询细节由子类实现
    }
}
```

## 建造者模式
**SqlSessionFactoryBuilder**：MyBatis 使用SqlSessionFactoryBuilder来构建SqlSessionFactory实例。

```plain
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

## 装饰器模式（Decorator Pattern）
**插件机制**：MyBatis 的插件机制使用了装饰器模式，可以在执行器、参数处理器等核心组件上添加额外的功能。

```plain
@Intercepts({@Signature(type = Executor.class, method = "update", args = {MappedStatement.class, Object.class})})
public class ExamplePlugin implements Interceptor {
    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        // 在执行器的 update 方法上添加额外的功能
    }
}
```

## 策略模式（Strategy Pattern）
**缓存策略**：MyBatis 的缓存机制使用了策略模式，不同的缓存实现可以互相替换。

```plain
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

## 责任链模式（Chain of Responsibility Pattern）
**拦截器链**：MyBatis 的拦截器机制使用了责任链模式，可以在执行 SQL 语句的过程中通过一系列拦截器进行处理。

```plain
public Object pluginAll(Object target) {
    for (Interceptor interceptor : interceptors) {
        target = interceptor.plugin(target);
    }
    return target;
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/vzy4cfmct1pecbq3>