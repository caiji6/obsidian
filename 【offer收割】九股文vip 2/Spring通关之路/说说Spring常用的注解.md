# 👌说说Spring常用的注解

# 口语化回答
好的，面试官，比较常用的就是Component，将类放到容器管理，autowired，resource，来装配 bean。还有就是 configuration 和 bean 进行配合，装载 bean 进入容器。以及一些扩展的注解比如 aop 的 aspect 切面，事务相关的 transsactional。以上。

# 题目解析
基本不会考，这个没啥意义，不排除小公司或应届会问到这个问题。

# 面试得分点
随便搞下面的 3～4 个注解说出来即可。

# 题目详细答案
## 核心注解
@Component：将一个类标记为 Spring 组件类，表示这个类的实例将由 Spring 容器管理。

@Autowired：自动注入依赖对象。可以用于构造器、字段、Setter 方法或其他任意方法。

@Qualifier：在有多个候选 Bean 时，指定要注入的 Bean。通常与@Autowired一起使用。

```plain
@Component
public class MyComponent {
    @Autowired
    @Qualifier("specificBean")
    private Dependency dependency;
}
```

@Value：注入外部化的值，例如配置文件中的属性值、系统属性或环境变量。

```plain
@Component
public class MyComponent {
    @Value("${my.property}")
    private String myProperty;
}
```

@Configuration：标记一个类为 Spring 配置类，该类可以包含一个或多个@Bean方法。

```plain
@Configuration
public class AppConfig {
    @Bean
    public MyComponent myComponent() {
        return new MyComponent();
    }
}
```

@Bean：定义一个方法，返回一个要注册为 Spring 容器管理的 Bean。

```plain
@Bean
public MyComponent myComponent() {
    return new MyComponent();
}
```

@ComponentScan：指定要扫描的包，以查找带有@Component、@Service、@Repository和@Controller注解的类，并将它们注册为 Spring Bean。

```plain
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类内容
}
```

@Primary：标记一个 Bean 为首选 Bean，当有多个候选 Bean 时，Spring 会优先注入带有@Primary注解的 Bean。

```plain
@Component
@Primary
public class PrimaryBean implements Dependency {
    // 实现
}
```

@Scope：指定 Bean 的作用域，例如单例（singleton）、原型（prototype）等。

```plain
@Component
@Scope("prototype")
public class PrototypeBean {
    // 实现
}
```

## 特定用途注解
@Service：标记一个类为服务层组件，实际上是@Component的特殊化。

```plain
@Service
public class MyService {
    // 服务逻辑
}
```

@Repository：标记一个类为数据访问层组件，实际上是@Component的特殊化，并且支持持久化异常转换。

```plain
@Repository
public class MyRepository {
    // 数据访问逻辑
}
```

@Controller：标记一个类为 Spring MVC 控制器，实际上是@Component的特殊化

```plain
@Controller
public class MyController {
    // 控制器逻辑
}
```

@RestController：标记一个类为 RESTful Web 服务的控制器，等同于@Controller和@ResponseBody的组合。

```plain
@RestController
public class MyRestController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

## AOP（面向切面编程）相关注解
@Aspect：标记一个类为切面类。

```plain
@Aspect
@Component
public class MyAspect {
    // 切面逻辑
}
```

@Before：定义一个方法，在目标方法执行之前执行。

```plain
@Aspect
@Component
public class MyAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void beforeMethod(JoinPoint joinPoint) {
        // 前置逻辑
    }
}
```

@After：定义一个方法，在目标方法执行之后执行。

```plain
@Aspect
@Component
public class MyAspect {
    @After("execution(* com.example.service.*.*(..))")
    public void afterMethod(JoinPoint joinPoint) {
        // 后置逻辑
    }
}
```

@Around：定义一个方法，包围目标方法的执行

```plain
@Aspect
@Component
public class MyAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object aroundMethod(ProceedingJoinPoint joinPoint) throws Throwable {
        // 环绕逻辑
        return joinPoint.proceed();
    }
}
```

## 事务管理相关注解
@Transactional：标记一个方法或类，表示该方法或类中的所有方法都需要事务管理。

```plain
@Service
public class MyService {
    @Transactional
    public void performTransaction() {
        // 事务逻辑
    }
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ubzdalzwa3ke8el0>