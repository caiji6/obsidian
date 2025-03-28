# 👌讲一讲@PropertySource

# 题目详细答案
`@PropertySource`用于指定外部属性文件并将其加载到 Spring 环境中。通过使用`@PropertySource`注解，可以将外部的`.properties`文件中的属性值注入到 Spring 的`Environment`中，从而可以在应用中使用这些属性。

## 基本用法
`@PropertySource`注解通常与`@Configuration`注解一起使用。

```plain
app.name=MyApp
app.version=1.0.0
```



```plain
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String appVersion;

    // Getters and setters

    public String getAppName() {
        return appName;
    }

    public String getAppVersion() {
        return appVersion;
    }
}
```

## 多个属性文件
`@PropertySource`注解支持加载多个属性文件，可以通过数组的形式指定多个文件。



```plain
@Configuration
@PropertySource({"classpath:application.properties", "classpath:additional.properties"})
public class AppConfig {
    // 属性注入代码
}
```

## 使用`Environment`获取属性
除了使用`@Value`注解外，还可以通过 Spring 的`Environment`接口来获取属性值。

```plain
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.env.Environment;

@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {

    @Autowired
    private Environment env;

    public String getAppName() {
        return env.getProperty("app.name");
    }

    public String getAppVersion() {
        return env.getProperty("app.version");
    }
}
```

## 支持的属性文件位置
`@PropertySource`支持各种属性文件位置，包括：

+ **类路径**：`classpath:application.properties`
+ **文件系统**：`file:/path/to/application.properties`
+ **URL**：`http://example.com/application.properties`

## 忽略资源不存在的情况
在某些情况下，属性文件可能不存在。可以通过`ignoreResourceNotFound`属性来忽略这种情况。

```plain
@Configuration
@PropertySource(value = "classpath:nonexistent.properties", ignoreResourceNotFound = true)
public class AppConfig {
    // 属性注入代码
}
```

## 使用`PropertySourcesPlaceholderConfigurer`
在某些情况下，特别是当你需要在`@Configuration`类中使用占位符时，可能需要显式地定义一个`PropertySourcesPlaceholderConfigurer`bean。

```plain
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;

@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {

    @Bean
    public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {
        return new PropertySourcesPlaceholderConfigurer();
    }

    // 属性注入代码
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/dn6ui6twnfielbdv>