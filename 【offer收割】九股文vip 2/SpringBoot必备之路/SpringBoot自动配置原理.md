# 👌SpringBoot自动配置原理

# 题目详细答案
Spring Boot 的自动配置,能够根据项目中的依赖和配置自动地为你配置 Spring 应用程序，而无需手动编写大量的配置代码。这个特性极大地简化了 Spring 应用程序的开发过程。

## 自动配置的基本原理
### 条件注解
Spring Boot 使用了一系列的条件注解（Conditional Annotations）来实现自动配置。这些注解包括：

`@ConditionalOnClass`：当类路径上存在指定的类时，配置才会生效。

`@ConditionalOnMissingClass`：当类路径上不存在指定的类时，配置才会生效。

`@ConditionalOnBean`：当 Spring 容器中存在指定的 Bean 时，配置才会生效。

`@ConditionalOnMissingBean`：当 Spring 容器中不存在指定的 Bean 时，配置才会生效。

`@ConditionalOnProperty`：当指定的属性存在或具有特定值时，配置才会生效。

`@ConditionalOnResource`：当类路径下存在指定的资源时，配置才会生效。

`@ConditionalOnWebApplication`：当当前应用是 Web 应用时，配置才会生效。

`@ConditionalOnNotWebApplication`：当当前应用不是 Web 应用时，配置才会生效。

### `@EnableAutoConfiguration`注解
`@EnableAutoConfiguration`是 Spring Boot 自动配置的核心注解，它通常与`@SpringBootApplication`一起使用。`@SpringBootApplication`是一个组合注解，包含了`@EnableAutoConfiguration`、`@ComponentScan`和`@Configuration`。

```plain
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### `spring.factories`文件
Spring Boot 使用`spring.factories`文件来列出所有的自动配置类。这个文件位于每个自动配置模块的`META-INF`目录下。Spring Boot 在启动时会扫描这些文件，并加载其中列出的自动配置类。

`spring.factories`文件的内容示例如下：

```plain
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
```

## 自动配置的工作流程
Spring Boot 自动配置的工作流程可以分为以下几个步骤：

1. **扫描**`**spring.factories**`**文件**：Spring Boot 启动时，会扫描所有依赖中的`spring.factories`文件，找到所有列出的自动配置类。
2. **加载自动配置类**：Spring Boot 会尝试加载这些自动配置类。
3. **评估条件注解**：对于每个自动配置类，Spring Boot 会评估其上的条件注解。如果所有条件都满足，则该自动配置类会被应用。
4. **应用自动配置**：自动配置类中的配置会被应用到 Spring 容器中，注册相应的 Bean。

## 示例：自动配置 DataSource
### `spring.factories`文件
在`spring-boot-autoconfigure`模块的`META-INF/spring.factories`文件中，列出了`DataSourceAutoConfiguration`：

```plain
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

### `DataSourceAutoConfiguration`类
`DataSourceAutoConfiguration`类使用了条件注解来判断是否需要自动配置 DataSource：

```plain
@Configuration
@ConditionalOnClass(DataSource.class)
@EnableConfigurationProperties(DataSourceProperties.class)
@Import({DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class})
public class DataSourceAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean
    @ConditionalOnSingleCandidate(DataSource.class)
    public DataSourceInitializer dataSourceInitializer(DataSource dataSource, DataSourceProperties properties) {
        return new DataSourceInitializer(dataSource, properties);
    }

    // 其他配置
}
```

### 条件注解的评估
在应用启动时，Spring Boot 会评估`DataSourceAutoConfiguration`类上的条件注解：

`@ConditionalOnClass(DataSource.class)`：确保类路径上存在`javax.sql.DataSource`类。

`@ConditionalOnMissingBean`：确保 Spring 容器中不存在其他类型为`DataSource`的 Bean。

如果这些条件都满足，Spring Boot 会应用`DataSourceAutoConfiguration`中的配置，注册相应的 DataSource Bean。

## 禁用自动配置
在某些情况下，你可能希望禁用某些自动配置。可以通过以下方式实现：

### 使用`@SpringBootApplication`注解的`exclude`属性
```plain
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### 使用`application.properties`文件
```plain
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hhc4cwpmnwncpdfq>