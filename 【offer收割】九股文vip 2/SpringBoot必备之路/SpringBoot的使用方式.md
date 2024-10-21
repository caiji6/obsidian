# 👌SpringBoot的使用方式

# 题目详细答案
Spring Boot 是一个用于快速构建基于 Spring 框架的独立、生产级应用程序的框架。它可以极大地简化 Spring 应用程序的开发过程，提供了一系列的默认配置和自动配置机制。

## 使用 Spring Initializr 创建项目
Spring Initializr 是一个在线工具，可以帮助你快速生成一个 Spring Boot 项目。你可以通过访问Spring Initializr网站，选择项目的基本配置（如 Maven/Gradle、Spring Boot 版本、依赖等），然后生成项目。

## 创建 Spring Boot 项目
可以使用 Spring Initializr 或者手动创建一个 Spring Boot 项目。以下是一个使用 Maven 创建 Spring Boot 项目的示例：

### 使用 Spring Initializr
访问Spring Initializr，选择项目的基本配置，如下：

Project: Maven Project

Language: Java

Spring Boot: 选择合适的版本（例如 2.7.5）

Project Metadata: 填写 Group、Artifact 等信息

Dependencies: 选择你需要的依赖（如 Spring Web）

点击 "Generate" 按钮下载生成的项目压缩包，解压后导入到你的 IDE 中。

### 手动创建项目
1. 创建一个新的 Maven 项目。
2. 在`pom.xml`文件中添加 Spring Boot 依赖：

```plain
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.5</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- 其他依赖 -->
</dependencies>
```

## 编写 Spring Boot 应用程序
### 创建主应用类
在你的项目中创建一个主应用类，并使用`@SpringBootApplication`注解标注：

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### 创建 REST 控制器
创建一个简单的 REST 控制器：

```plain
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

## 运行 Spring Boot 应用程序
### 使用 IDE
在你的 IDE 中运行主应用类`DemoApplication`，Spring Boot 应用程序将会启动，并嵌入一个 Tomcat 服务器。

### 使用 Maven
在项目根目录下运行以下命令：

```plain
mvn spring-boot:run
```

### 生成可执行 JAR 文件
你可以生成一个可执行的 JAR 文件，并通过命令行运行：

```plain
mvn clean package
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

## 配置 Spring Boot 应用程序
Spring Boot 支持多种配置方式，你可以在`src/main/resources`目录下创建`application.properties`或者`application.yml`文件，进行配置。例如：

#### `application.properties`
```plain
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=secret
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/rd6ax5e8syi16z6d>