# 👌Spring中用到了哪些设计模式

# 口语化回答
好的，面试官，设计模式在 spring 中有很多的体现，比如工厂模式，spring 的 applicationtext 就是 spring 实现工厂模式的典型例子，以及 spring 容器中，默认每个 bean 都是单例的。aop 的面向切面则是体现的代理模式。还有就是 spring 的 event 的事件驱动，采取了观察者模式。其他的还有装饰器，策略模式等，以上。

# 题目解析
问的还是挺常见，经常和设计模式一起问。一般是先问设计模式然后延伸到 spring 或者其他框架都有什么。

# 面试得分点
下面这些模式，全部得分。

# 题目详细答案
## 工厂模式（Factory Pattern）
Spring 使用工厂模式来创建对象实例。BeanFactory和ApplicationContext是 Spring 框架中实现工厂模式的核心接口。

**应用场景**：Spring 容器负责创建和管理 bean 的实例。

```plain
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
MyBean myBean = (MyBean) context.getBean("myBean");
```

## 单例模式（Singleton Pattern）
Spring 默认以单例模式管理 bean，这意味着每个 bean 在 Spring 容器中只有一个实例。

**应用场景**：默认情况下，Spring 容器中的每个 bean 都是单例的。

```plain
@Component
public class MySingletonBean {
    // This bean will be a singleton
}
```

## 代理模式（Proxy Pattern）
Spring AOP（面向切面编程）使用代理模式来创建代理对象，以实现方法拦截和增强功能。

**应用场景**：AOP 实现事务管理、日志记录、安全检查等。

```plain
@Aspect
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBeforeMethod(JoinPoint joinPoint) {
        System.out.println("Executing method: " + joinPoint.getSignature().getName());
    }
}
```

## 模板方法模式（Template Method Pattern）
Spring 提供了多种模板类（如JdbcTemplate、RestTemplate），这些类封装了常见的操作步骤，允许用户只需实现特定的步骤。

**应用场景**：简化数据库操作、RESTful 服务调用等。

```plain
@Autowired
private JdbcTemplate jdbcTemplate;

public void saveData(String data) {
    String sql = "INSERT INTO my_table (data) VALUES (?)";
    jdbcTemplate.update(sql, data);
}
```

## 观察者模式（Observer Pattern）
Spring 的事件处理机制使用观察者模式。ApplicationEventPublisher和ApplicationListener是实现观察者模式的核心接口。

**应用场景**：实现事件驱动的编程模型。

```plain
public class MyEvent extends ApplicationEvent {
    public MyEvent(Object source) {
        super(source);
    }
}

@Component
public class MyEventListener implements ApplicationListener<MyEvent> {
    @Override
    public void onApplicationEvent(MyEvent event) {
        System.out.println("Received event: " + event.getSource());
    }
}

@Component
public class MyEventPublisher {
    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publishEvent() {
        MyEvent event = new MyEvent(this);
        applicationEventPublisher.publishEvent(event);
    }
}
```

## 依赖注入模式（Dependency Injection Pattern）
Spring 的核心功能之一就是依赖注入，通过构造函数注入、setter 注入或字段注入，将对象的依赖关系注入到对象中。

**应用场景**：解耦对象之间的依赖关系，便于测试和维护。

```plain
@Component
public class MyService {
    private final MyRepository myRepository;

    @Autowired
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```

## 装饰器模式（Decorator Pattern）
Spring 使用装饰器模式来增强 bean 的功能，特别是在 AOP 中，通过将增强逻辑应用到目标对象上。

**应用场景**：动态地为对象添加职责，而不影响其他对象。

```plain
@Aspect
public class PerformanceAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println("Method executed in: " + executionTime + "ms");
        return result;
    }
}
```

## 策略模式（Strategy Pattern）
Spring 中的TransactionManager使用策略模式来定义事务管理的策略，允许在运行时选择不同的事务管理器。

**应用场景**：定义一系列算法，允许在运行时选择具体的算法。

```plain
@Configuration
public class AppConfig {
    @Bean
    public PlatformTransactionManager transactionManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/qw0m2oxbfy66q73r>