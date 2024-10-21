# 👌SpringBoot如何瘦身

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">主要目标是减少应用程序的启动时间、内存占用和打包大小。</font>

## <font style="background-color:rgb(247, 248, 249);">减少依赖</font>
**<font style="background-color:rgb(247, 248, 249);">移除不必要的依赖</font>**<font style="background-color:rgb(247, 248, 249);">：检查</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);">或</font>`<font style="background-color:rgb(247, 248, 249);">build.gradle</font>`<font style="background-color:rgb(247, 248, 249);">文件，移除那些不必要的依赖。</font>

**<font style="background-color:rgb(247, 248, 249);">精简依赖</font>**<font style="background-color:rgb(247, 248, 249);">：使用</font>`<font style="background-color:rgb(247, 248, 249);">spring-boot-starter</font>`<font style="background-color:rgb(247, 248, 249);">提供的精简依赖，比如</font>`<font style="background-color:rgb(247, 248, 249);">spring-boot-starter-web</font>`<font style="background-color:rgb(247, 248, 249);">可能包含了很多你不需要的内容，可以考虑使用更具体的依赖。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 ProGuard 或类似工具</font>
**<font style="background-color:rgb(247, 248, 249);">ProGuard</font>**<font style="background-color:rgb(247, 248, 249);">：可以通过混淆和优化来减小 JAR 包的大小。</font>

**<font style="background-color:rgb(247, 248, 249);">Maven ProGuard 插件</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

```plain
<build>
    <plugins>
        <plugin>
            <groupId>com.github.wvengen</groupId>
            <artifactId>proguard-maven-plugin</artifactId>
            <version>2.0.16</version>
            <executions>
                <execution>
                    <goals>
                        <goal>proguard</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
                <options>
                    <option>-dontwarn</option>
                    <option>-dontnote</option>
                    <option>-dontoptimize</option>
                    <option>-dontshrink</option>
                    <option>-dontobfuscate</option>
                </options>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## <font style="background-color:rgb(247, 248, 249);">使用 Spring Boot 2.x 提供的 Layered JAR</font>
**<font style="background-color:rgb(247, 248, 249);">Layered JAR</font>**<font style="background-color:rgb(247, 248, 249);">：Spring Boot 2.3 引入了分层 JAR，可以将应用程序分成多个层，便于缓存和优化 Docker 镜像。</font>

**<font style="background-color:rgb(247, 248, 249);">配置 Layered JAR</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

```plain
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <layers>
                    <enabled>true</enabled>
                </layers>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## <font style="background-color:rgb(247, 248, 249);">优化 Spring Boot 配置</font>
**<font style="background-color:rgb(247, 248, 249);">调整 JVM 参数</font>**<font style="background-color:rgb(247, 248, 249);">：根据应用程序的需求调整 JVM 参数，以优化内存使用和性能。</font>

```plain
java -Xms256m -Xmx512m -jar myapp.jar
```

**<font style="background-color:rgb(247, 248, 249);">使用 Spring Boot 的资源优化配置</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

```plain
spring.main.lazy-initialization=true
```

## <font style="background-color:rgb(247, 248, 249);">使用 Docker 多阶段构建</font>
**<font style="background-color:rgb(247, 248, 249);">多阶段构建</font>**<font style="background-color:rgb(247, 248, 249);">：减少 Docker 镜像的大小。</font>

```plain
FROM maven:3.8.1-openjdk-11 AS build
COPY src /home/app/src
COPY pom.xml /home/app
RUN mvn -f /home/app/pom.xml clean package

FROM openjdk:11-jre-slim
COPY --from=build /home/app/target/myapp.jar /usr/local/lib/myapp.jar
ENTRYPOINT ["java","-jar","/usr/local/lib/myapp.jar"]
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fcbzggrzgarzpomr>