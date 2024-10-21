# 👌什么是Spring Cloud Config

<font style="background-color:rgb(247, 248, 249);">Spring Cloud Config 是一个为分布式系统中的外部配置管理提供支持的工具。它允许你将应用的配置集中管理，并在运行时动态地更新这些配置，从而实现配置的集中化和动态化管理。Spring Cloud Config 提供了一个服务器和客户端的实现，分别用于管理和消费配置。</font>

### <font style="background-color:rgb(247, 248, 249);">核心组件</font>
1. **<font style="background-color:rgb(247, 248, 249);">Config Server</font>**<font style="background-color:rgb(247, 248, 249);">:</font>
    - <font style="background-color:rgb(247, 248, 249);">Config Server 是一个独立的应用程序，负责从配置存储库（如 Git、SVN 或本地文件系统）中读取配置文件，并将其提供给客户端应用程序。</font>
    - <font style="background-color:rgb(247, 248, 249);">配置存储库可以是一个 Git 仓库、SVN 仓库、Consul、Vault 等。</font>
2. **<font style="background-color:rgb(247, 248, 249);">Config Client</font>**<font style="background-color:rgb(247, 248, 249);">:</font>
    - <font style="background-color:rgb(247, 248, 249);">Config Client 是使用 Spring Cloud Config 的应用程序，它从 Config Server 获取配置。</font>
    - <font style="background-color:rgb(247, 248, 249);">客户端应用程序在启动时会从 Config Server 获取它们的配置，并在运行时可以动态刷新这些配置。</font>

### <font style="background-color:rgb(247, 248, 249);">主要特性</font>
1. **<font style="background-color:rgb(247, 248, 249);">集中化配置管理</font>**<font style="background-color:rgb(247, 248, 249);">:</font>
    - <font style="background-color:rgb(247, 248, 249);">将所有应用的配置文件集中存储在一个地方，方便管理和维护。</font>
2. **<font style="background-color:rgb(247, 248, 249);">环境特定的配置</font>**<font style="background-color:rgb(247, 248, 249);">:</font>
    - <font style="background-color:rgb(247, 248, 249);">支持根据不同的环境（如开发、测试、生产）提供不同的配置文件。</font>
3. **<font style="background-color:rgb(247, 248, 249);">动态刷新配置</font>**<font style="background-color:rgb(247, 248, 249);">:</font>
    - <font style="background-color:rgb(247, 248, 249);">支持在运行时动态刷新配置，而不需要重启应用。</font>
4. **<font style="background-color:rgb(247, 248, 249);">版本控制</font>**<font style="background-color:rgb(247, 248, 249);">:</font>
    - <font style="background-color:rgb(247, 248, 249);">配置文件可以存储在版本控制系统中（如 Git），方便追踪配置的变更历史。</font>

### <font style="background-color:rgb(247, 248, 249);">配置示例</font>
#### <font style="background-color:rgb(247, 248, 249);">1. 创建 Config Server</font>
<font style="background-color:rgb(247, 248, 249);">创建一个 Spring Boot 应用并添加 Spring Cloud Config Server 的依赖：</font>

```plain
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

<font style="background-color:rgb(247, 248, 249);">在主应用类上添加 </font>`<font style="background-color:rgb(247, 248, 249);">@EnableConfigServer</font>`<font style="background-color:rgb(247, 248, 249);"> 注解：</font>

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

<font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中配置配置存储库的位置，例如使用 Git：</font>

```plain
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-repo/config-repo
          searchPaths: config
```

#### <font style="background-color:rgb(247, 248, 249);">2. 创建 Config Client</font>
<font style="background-color:rgb(247, 248, 249);">创建另一个 Spring Boot 应用并添加 Spring Cloud Config Client 的依赖：</font>

```plain
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

<font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">bootstrap.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中配置 Config Server 的位置：</font>

```plain
spring:
  application:
    name: my-app
  cloud:
    config:
      uri: http://localhost:8888
```

<font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中，可以添加一些默认配置：</font>

```plain
server:
  port: 8080

my:
  property: default-value
```

<font style="background-color:rgb(247, 248, 249);">在 Git 仓库中，创建一个名为</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">my-app.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">的配置文件：</font>

```plain
my:
  property: value-from-config-server
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

### <font style="background-color:rgb(247, 248, 249);">总结</font>
<font style="background-color:rgb(247, 248, 249);">Spring Cloud Config 提供了一种集中化和动态化管理分布式系统配置的解决方案。通过 Config Server 和 Config Client 的配合，可以实现配置的集中管理、版本控制和动态刷新，从而简化配置管理，提高系统的灵活性和可维护性。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/mkxac8pg1xa7g368>