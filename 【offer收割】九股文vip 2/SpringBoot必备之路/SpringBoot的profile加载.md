# 👌SpringBoot的profile加载

# 题目详细答案
Spring Boot 的 Profile 是一种配置机制，用于在不同的环境中使用不同的配置。通过使用 Profile，可以在开发、测试、生产等不同环境中轻松切换配置，而无需修改代码或重新打包应用程序。

## 什么是 Profile
Profile 是 Spring 提供的一种功能，允许为不同的环境定义不同的 Bean 和配置。Spring Boot 通过`application-{profile}.properties`文件和`@Profile`注解来支持这种配置机制。

## 配置 Profile
## 使用`application-{profile}.properties`文件
在 Spring Boot 项目中，可以为不同的 Profile 创建不同的配置文件。例如：

+ `application-dev.properties`：开发环境的配置文件
+ `application-test.properties`：测试环境的配置文件
+ `application-prod.properties`：生产环境的配置文件

默认的`application.properties`文件可以包含所有环境通用的配置。

### 不同环境的配置文件
```plain
spring.datasource.url=jdbc:mysql://localhost:3306/devdb
spring.datasource.username=devuser
spring.datasource.password=devpass
```

```plain
spring.datasource.url=jdbc:mysql://localhost:3306/proddb
spring.datasource.username=produser
spring.datasource.password=prodpass
```

## 使用`@Profile`注解
可以在 Java 配置类或 Bean 定义上使用`@Profile`注解，以便在特定的 Profile 激活时启用这些配置。

```plain
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@Configuration
public class DataSourceConfig {

    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        // 配置开发环境的数据源
    }

    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        // 配置生产环境的数据源
    }
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/dyutyzx24fkp86mh>