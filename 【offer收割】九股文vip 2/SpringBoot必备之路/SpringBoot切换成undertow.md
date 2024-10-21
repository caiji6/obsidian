# 👌SpringBoot切换成undertow

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">Spring Boot 默认使用 Tomcat 作为嵌入式的 Servlet 容器，但你也可以切换到 Undertow。Undertow 是一个轻量级、高性能的 Web 服务器和 Servlet 容器。</font>

## <font style="background-color:rgb(247, 248, 249);">步骤 1：排除 Tomcat 依赖</font>
<font style="background-color:rgb(247, 248, 249);">需要在</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果使用的是 Maven）或</font>`<font style="background-color:rgb(247, 248, 249);">build.gradle</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果使用的是 Gradle）中排除 Tomcat 依赖。</font>

#### <font style="background-color:rgb(247, 248, 249);">Maven:</font>
```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

#### <font style="background-color:rgb(247, 248, 249);">Gradle:</font>
```plain
implementation('org.springframework.boot:spring-boot-starter-web') {
    exclude group: 'org.springframework.boot', module: 'spring-boot-starter-tomcat'
}
```

## <font style="background-color:rgb(247, 248, 249);">步骤 2：添加 Undertow 依赖</font>
<font style="background-color:rgb(247, 248, 249);">添加 Undertow 依赖。</font>

#### <font style="background-color:rgb(247, 248, 249);">Maven:</font>
```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```

## <font style="background-color:rgb(247, 248, 249);">步骤 3：确认配置</font>
<font style="background-color:rgb(247, 248, 249);">确保你的 Spring Boot 应用程序的主类没有特定于 Tomcat 的配置。通常情况不需要做任何修改，因为 Spring Boot 会自动配置 Undertow。</font>

### <font style="background-color:rgb(247, 248, 249);">示例</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`


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
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-undertow</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
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

## <font style="background-color:rgb(247, 248, 249);">验证</font>
<font style="background-color:rgb(247, 248, 249);">启动Spring Boot 应用程序。应该能够在控制台日志中看到 Undertow 服务器启动的消息，而不是 Tomcat。</font>

```plain
2021-08-30 10:00:00.000  INFO 12345 --- [           main] io.undertow                                : starting undertow server
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/enrbqxdey7qgz2ue>