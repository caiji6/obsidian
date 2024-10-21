# 👌Spring Cloud Config客户端读取配置文件

<font style="background-color:rgb(247, 248, 249);">Spring Cloud Config 客户端读取配置文件的过程涉及到从 Spring Cloud Config Server 获取配置，并将这些配置应用到客户端应用程序中。</font>

### <font style="background-color:rgb(247, 248, 249);">配置步骤</font>
#### <font style="background-color:rgb(247, 248, 249);">1. 创建 Spring Cloud Config Server</font>
<font style="background-color:rgb(247, 248, 249);">假设你已经有一个 Spring Cloud Config Server，它从 GitHub 仓库中读取配置文件。配置步骤如下：</font>

1. <font style="background-color:rgb(247, 248, 249);">创建一个新的 Spring Boot 应用程序，并添加 Spring Cloud Config Server 的依赖：</font>

```plain
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

2. <font style="background-color:rgb(247, 248, 249);">在主应用类上添加</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@EnableConfigServer</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解：</font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

3. <font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中配置 GitHub 仓库的 URI 和分支：</font>

```plain
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-username/config-repo
          searchPaths: config
          clone-on-start: true
          username: your-github-username
          password: your-github-token
```

#### <font style="background-color:rgb(247, 248, 249);">2. 创建 Spring Cloud Config 客户端</font>
1. <font style="background-color:rgb(247, 248, 249);">创建一个新的 Spring Boot 应用程序，并添加 Spring Cloud Config Client 的依赖：</font>

```plain
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

2. <font style="background-color:rgb(247, 248, 249);">在 </font>`<font style="background-color:rgb(247, 248, 249);">bootstrap.yml</font>`<font style="background-color:rgb(247, 248, 249);"> 文件中配置 Config Server 的 URI 和应用名称：</font>

```plain
spring:
  application:
    name: my-app
  cloud:
    config:
      uri: http://localhost:8888
```

    - `<font style="background-color:rgb(247, 248, 249);">spring.application.name</font>`<font style="background-color:rgb(247, 248, 249);">：指定应用程序的名称，这个名称将用于从 Config Server 获取对应的配置文件。</font>
    - `<font style="background-color:rgb(247, 248, 249);">spring.cloud.config.uri</font>`<font style="background-color:rgb(247, 248, 249);">：指定 Config Server 的 URI。</font>
3. <font style="background-color:rgb(247, 248, 249);">在 GitHub 仓库中，创建一个名为</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">my-app.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">的配置文件：</font>

```plain
my:
  property: value-from-config-server
```

4. <font style="background-color:rgb(247, 248, 249);">在客户端应用程序中，可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@Value</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解获取配置属性：</font>

```plain
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @Value("${my.property}")
    private String myProperty;

    @GetMapping("/property")
    public String getProperty() {
        return myProperty;
    }
}
```

#### <font style="background-color:rgb(247, 248, 249);">3. 动态刷新配置</font>
<font style="background-color:rgb(247, 248, 249);">为了支持动态刷新配置，可以添加 Spring Boot Actuator 和 Spring Cloud Bus 的依赖：</font>

```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

<font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中启用 Actuator 的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">/refresh</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">端点：</font>

```plain
management:
  endpoints:
    web:
      exposure:
        include: refresh
```

<font style="background-color:rgb(247, 248, 249);">在需要动态刷新的类上添加</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RefreshScope</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解：</font>

```plain
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RefreshScope
@RestController
public class MyController {

    @Value("${my.property}")
    private String myProperty;

    @GetMapping("/property")
    public String getProperty() {
        return myProperty;
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gt6bxr12wz08pny0>