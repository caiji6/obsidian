# 👌ApplicationContext的实现类有哪些？

# 口语化答案
好的，面试官。常用的只要用 classpathxml 的基于 xml 的方式，annotion 的基于注解的方式，不常见的还有 web 和 groovy。在目前的实际情况下，主要是 annotion，注解形式放入到容器中。像老的 tomcat 那种方式，用的是 web 形式，不过现在都是 boot 注解形式够用。groovy 适用于一些动态加载 bean 的方式，通过脚本的形式处理。以上。

# 题目解析
经典题，问的还比较多，主要是想借此考察一下，你在项目中，都用过什么样的形式的。其实主要答出注解和 xml 这道题就 ok。

# 面试得分点
xml，注解，web，groovy

# 题目详细答案
## 基于XML配置的实现类
**ClassPathXmlApplicationContext：**从类路径下加载XML配置文件。

```plain
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

**FileSystemXmlApplicationContext：**从文件系统路径加载XML配置文件。

```plain
ApplicationContext context = new FileSystemXmlApplicationContext("C:/path/to/applicationContext.xml");
```

## 基于注解配置的实现类
**AnnotationConfigApplicationContext：**从Java配置类（使用@Configuration注解的类）加载配置。

```plain
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}

ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
```

## 基于Web应用的实现类
**XmlWebApplicationContext：**专门为Web应用设计的ApplicationContext实现类，从Web应用的上下文中加载XML配置文件。

通常在web.xml中配置：

```plain
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/applicationContext.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

**AnnotationConfigWebApplicationContext：**专门为Web应用设计的ApplicationContext实现类，从Java配置类加载配置

```plain
public class WebAppInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext container) {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(AppConfig.class);
        container.addListener(new ContextLoaderListener(context));
    }
}
```

## 基于Groovy配置的实现类
**GenericGroovyApplicationContext：**从Groovy脚本配置文件加载配置。

```plain
ApplicationContext context = new GenericGroovyApplicationContext("applicationContext.groovy");
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ogqocf1wzzfkvcd6>