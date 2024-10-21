# 👌SpringBoot的监控

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring Boot 应用程序中，监控是确保应用程序健康运行和性能优化的重要部分。Spring Boot 提供了多种监控和管理功能，其中最常用的是 Spring Boot Actuator 和集成外部监控工具（如 Prometheus 和 Grafana）。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 Spring Boot Actuator</font>
<font style="background-color:rgb(247, 248, 249);">Spring Boot Actuator 提供了一组内置的端点，用于监控和管理应用程序。它可以帮助你查看应用程序的健康状况、度量指标、环境信息等。</font>

### <font style="background-color:rgb(247, 248, 249);">步骤 1：添加依赖</font>
<font style="background-color:rgb(247, 248, 249);">在你的</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果你使用的是 Maven）或</font>`<font style="background-color:rgb(247, 248, 249);">build.gradle</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果你使用的是 Gradle）中添加 Spring Boot Actuator 依赖。</font>

##### <font style="background-color:rgb(247, 248, 249);">Maven:</font>
```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### <font style="background-color:rgb(247, 248, 249);">步骤 2：配置 Actuator 端点</font>
<font style="background-color:rgb(247, 248, 249);">你可以在</font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/application.properties</font>`<font style="background-color:rgb(247, 248, 249);">文件中配置 Actuator 端点。</font>

```plain
# 启用所有 Actuator 端点
management.endpoints.web.exposure.include=*
```

### <font style="background-color:rgb(247, 248, 249);">常用 Actuator 端点</font>
`<font style="background-color:rgb(247, 248, 249);">/actuator/health</font>`<font style="background-color:rgb(247, 248, 249);">: 显示应用程序的健康状况。</font>

`<font style="background-color:rgb(247, 248, 249);">/actuator/info</font>`<font style="background-color:rgb(247, 248, 249);">: 显示应用程序的基本信息。</font>

`<font style="background-color:rgb(247, 248, 249);">/actuator/metrics</font>`<font style="background-color:rgb(247, 248, 249);">: 显示应用程序的度量指标。</font>

`<font style="background-color:rgb(247, 248, 249);">/actuator/loggers</font>`<font style="background-color:rgb(247, 248, 249);">: 显示和配置日志记录级别。</font>

`<font style="background-color:rgb(247, 248, 249);">/actuator/env</font>`<font style="background-color:rgb(247, 248, 249);">: 显示当前环境属性。</font>

#### <font style="background-color:rgb(247, 248, 249);">示例：</font>
<font style="background-color:rgb(247, 248, 249);">启动应用程序后，你可以通过以下 URL 访问 Actuator 端点：</font>

```plain
http://localhost:8080/actuator/health
http://localhost:8080/actuator/info
http://localhost:8080/actuator/metrics
```

## <font style="background-color:rgb(247, 248, 249);">使用 Prometheus 和 Grafana 进行监控</font>
<font style="background-color:rgb(247, 248, 249);">Prometheus 是一个开源的系统监控和报警工具，Grafana 是一个开源的分析和监控平台。它们常常一起使用来监控 Spring Boot 应用程序。</font>

#### <font style="background-color:rgb(247, 248, 249);">步骤 1：添加依赖</font>
<font style="background-color:rgb(247, 248, 249);">在你的</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);">文件中添加 Micrometer Prometheus 依赖：</font>

```plain
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

#### <font style="background-color:rgb(247, 248, 249);">步骤 2：配置 Prometheus 端点</font>
<font style="background-color:rgb(247, 248, 249);">在</font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/application.properties</font>`<font style="background-color:rgb(247, 248, 249);">文件中配置 Prometheus 端点：</font>

```plain
management.endpoints.web.exposure.include=prometheus
```

#### <font style="background-color:rgb(247, 248, 249);">步骤 3：配置 Prometheus</font>
<font style="background-color:rgb(247, 248, 249);">在 Prometheus 配置文件中添加你的 Spring Boot 应用程序作为一个抓取目标。</font>

```plain
scrape_configs:
  - job_name: 'spring-boot'
    static_configs:
      - targets: ['localhost:8080']
```

#### <font style="background-color:rgb(247, 248, 249);">步骤 4：配置 Grafana</font>
<font style="background-color:rgb(247, 248, 249);">在 Grafana 中添加 Prometheus 数据源，并创建仪表盘来可视化你的应用程序指标。</font>

## <font style="background-color:rgb(247, 248, 249);">demo</font>
#### `<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`
```plain
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>
    <description>Demo project for Spring Boot</description>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-prometheus</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

#### `<font style="background-color:rgb(247, 248, 249);">MyApplication.java</font>`
```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

#### `<font style="background-color:rgb(247, 248, 249);">application.properties</font>`
```plain
management.endpoints.web.exposure.include=*
management.endpoint.prometheus.enabled=true
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/zmlwug69knrgg76r>