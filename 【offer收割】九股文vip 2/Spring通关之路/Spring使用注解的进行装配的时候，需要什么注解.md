# 👌Spring使用注解的进行装配的时候，需要什么注解

# 口语化答案
好的，面试官，其实这个问题的目的就是将 bean 让 spring 扫描到，并识别出要放到容器中。那就涉及到第一个概念，ComponentScan，自动扫描指定的包，配合basePackages 属性。指定包之后能，spring 就会将@Service、@Repository和@Controller 这些带了注解的类，作为 bean 放入到容器中。ComponentScan 也提供了一些比如 excludefilter 想排除某些包，或者加特定包的方式。以上。

# 题目解析
基础题必看，主要稍微会发散的一个点就是 lazyinit，其他的没有什么东西。正常背一下属性就可以了。

# 面试得分点
basepackage，includefilter，扫描注解

# 题目详细答案
@ComponentScan是用于自动扫描指定的包及其子包中的组件类，并将它们注册为 Spring 容器管理的 Bean。这个过程被称为组件扫描（Component Scanning）。

## @ComponentScan注解
@ComponentScan注解通常与@Configuration注解一起使用，用于定义 Spring 应用的配置类。在这个配置类中，@ComponentScan指定了要扫描的包路径，Spring 会自动扫描这些包及其子包中的所有类，并将带有特定注解的类（如@Component、@Service、@Repository、@Controller等）注册为 Bean。

```plain
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类内容
}
```

Spring 会扫描com.example包及其子包中的所有类，并将带有@Component、@Service、@Repository和@Controller注解的类注册为 Spring Bean。

## 详细说明
#### basePackages属性
basePackages属性用于指定要扫描的包。可以指定一个或多个包路径。

```plain
@ComponentScan(basePackages = {"com.example", "com.another"})
```

#### basePackageClasses属性
basePackageClasses属性用于指定一个或多个类，Spring 会扫描这些类所在的包。

```plain
@ComponentScan(basePackageClasses = {MyClass1.class, MyClass2.class})
```

#### includeFilters和excludeFilters属性
includeFilters和excludeFilters属性用于指定包含或排除的过滤器。可以根据注解、类型、正则表达式等进行过滤。

```plain
@ComponentScan(
    basePackages = "com.example",
    includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = CustomAnnotation.class),
    excludeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = "com\\.example\\.exclude\\..*")
)
```

#### lazyInit属性
lazyInit属性用于指定是否延迟初始化扫描到的 Bean。默认值为false，表示立即初始化。

```plain
@ComponentScan(basePackages = "com.example", lazyInit = true)
```

## 示例
假设我们有以下包结构：

```plain
com.example
    ├── service
    │   └── MyService.java
    ├── repository
    │   └── MyRepository.java
    └── controller
        └── MyController.java
```

其中，MyService.java、MyRepository.java和MyController.java分别带有@Service、@Repository和@Controller注解。

#### 定义配置类
```plain
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类内容
}
```

#### 启动 Spring 容器
```plain
public class Application {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        
        MyService myService = context.getBean(MyService.class);
        MyRepository myRepository = context.getBean(MyRepository.class);
        MyController myController = context.getBean(MyController.class);
        
        // 使用 Bean
        context.close();
    }
}
```

Spring 会扫描com.example包及其子包中的所有类，并将带有@Service、@Repository和@Controller注解的类注册为 Spring Bean。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/xes2apguba92t7xp>