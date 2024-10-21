# 👌SpringBoot激活指定profile的几种方式

# 题目详细答案
在 Spring Boot 中，可以通过多种方式激活指定的 Profile，以便在不同的环境中使用不同的配置。

## 在`application.properties`文件中激活
可以在默认的`application.properties`文件中通过`spring.profiles.active`属性激活某个 Profile。

```plain
# application.properties
spring.profiles.active=dev
```

## 通过命令行参数激活
在启动 Spring Boot 应用程序时，可以通过命令行参数激活某个 Profile。

```plain
java -jar myapp.jar --spring.profiles.active=prod
```

## 通过环境变量激活
可以通过设置环境变量来激活某个 Profile。

```plain
export SPRING_PROFILES_ACTIVE=prod
```

## 通过 JVM 系统属性激活
可以通过设置 JVM 系统属性来激活某个 Profile。

```plain
java -Dspring.profiles.active=prod -jar myapp.jar
```

## 通过编程方式激活
可以在应用程序的启动类中通过编程方式激活 Profile。



```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.core.env.AbstractEnvironment;

@SpringBootApplication
public class MyApp {

    public static void main(String[] args) {
        System.setProperty(AbstractEnvironment.ACTIVE_PROFILES_PROPERTY_NAME, "dev");
        SpringApplication.run(MyApp.class, args);
    }
}
```

## 通过`@ActiveProfiles`注解激活（仅用于测试）
在测试类中，可以使用`@ActiveProfiles`注解来激活指定的 Profile。

```plain
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ActiveProfiles;

@SpringBootTest
@ActiveProfiles("test")
public class MyAppTests {

    @Test
    void contextLoads() {
        // 测试代码
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ckxyzx3gmzbldsom>