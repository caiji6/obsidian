# 👌SpringBoot热部署

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">热部署（Hot Deployment）可以显著提高开发效率。热部署允许你在不重启服务器的情况下看到代码更改的效果。Spring Boot 提供了多种方式来实现热部署，最常用的方法是使用 Spring Boot DevTools 和 JRebel。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 Spring Boot DevTools</font>
<font style="background-color:rgb(247, 248, 249);">Spring Boot DevTools 是一个专门为开发环境设计的工具，它提供了自动重启、LiveReload、配置属性覆盖等功能。</font>

### <font style="background-color:rgb(247, 248, 249);">添加依赖</font>
<font style="background-color:rgb(247, 248, 249);">在你的</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果你使用的是 Maven）或</font>`<font style="background-color:rgb(247, 248, 249);">build.gradle</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果你使用的是 Gradle）中添加 Spring Boot DevTools 依赖。</font>

##### <font style="background-color:rgb(247, 248, 249);">Maven:</font>
```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <font style="background-color:rgb(247, 248, 249);">启用自动重启</font>
<font style="background-color:rgb(247, 248, 249);">Spring Boot DevTools 默认启用自动重启功能。当你更改类路径中的文件时，应用程序会自动重启。</font>

##### <font style="background-color:rgb(247, 248, 249);">配置文件（可选）：</font>
<font style="background-color:rgb(247, 248, 249);">你可以在</font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/application.properties</font>`<font style="background-color:rgb(247, 248, 249);">文件中配置 DevTools 的一些属性。</font>

```plain
# 禁用自动重启
spring.devtools.restart.enabled=false

# 启用LiveReload
spring.devtools.livereload.enabled=true
```

#### <font style="background-color:rgb(247, 248, 249);">使用 LiveReload</font>
<font style="background-color:rgb(247, 248, 249);">LiveReload 功能允许浏览器在资源（如 HTML、CSS、JavaScript）更改时自动刷新页面。你需要安装一个 LiveReload 浏览器扩展来使用此功能。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 JRebel</font>
<font style="background-color:rgb(247, 248, 249);">JRebel 是一个商业工具，提供更高级的热部署功能。它不仅支持 Spring Boot，还支持其他 Java 框架和应用服务器。</font>

#### <font style="background-color:rgb(247, 248, 249);">步骤 1：安装 JRebel</font>
<font style="background-color:rgb(247, 248, 249);">你需要从 JRebel 官网下载并安装 JRebel 插件。JRebel 提供 Eclipse、IntelliJ IDEA 和其他主要 IDE 的插件。</font>

#### <font style="background-color:rgb(247, 248, 249);">步骤 2：配置 JRebel</font>
<font style="background-color:rgb(247, 248, 249);">安装 JRebel 插件后，你需要在 IDE 中启用 JRebel 并为你的项目配置 JRebel。具体步骤因 IDE 而异，请参考 JRebel 的官方文档。</font>

#### <font style="background-color:rgb(247, 248, 249);">步骤 3：运行应用程序</font>
<font style="background-color:rgb(247, 248, 249);">使用 JRebel 运行你的 Spring Boot 应用程序。JRebel 会监视你的代码更改并即时应用，不需要重启服务器。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ggb1q4ry9rdc0mih>