# 👌Spring MVC 与 Spring Boot 有什么区别？

# 题目详细答案
## <font style="background-color:rgb(247, 248, 249);">Spring MVC</font>
<font style="background-color:rgb(247, 248, 249);">Spring MVC 是一个基于模型-视图-控制器（MVC）设计模式的 Web 应用程序框架。它提供了一种灵活的方式来构建 Web 应用程序，主要关注于处理 HTTP 请求和生成响应。Spring MVC 的核心是 DispatcherServlet，它负责将请求路由到相应的控制器，并将控制器返回的视图转换为适当的响应格式。</font>

<font style="background-color:rgb(247, 248, 249);">特点：</font>

<font style="background-color:rgb(247, 248, 249);">提供了一个清晰的分层结构，明确区分了模型、视图和控制器的职责。</font>

<font style="background-color:rgb(247, 248, 249);">支持多种视图技术，包括 JSP、FreeMarker、Velocity 和 Thymeleaf 等。</font>

<font style="background-color:rgb(247, 248, 249);">允许使用注解来简化控制器的编写，例如 @Controller、@RequestMapping 和 @RequestParam 等。</font>

<font style="background-color:rgb(247, 248, 249);">可以与其他 Spring Framework 组件无缝集成，例如 Spring Security 和 Spring Data。</font>

## <font style="background-color:rgb(247, 248, 249);">Spring Boot</font>
<font style="background-color:rgb(247, 248, 249);">Spring Boot 是一个开箱即用的框架，旨在简化新 Spring 基于应用程序的初始搭建和开发。它提供了一系列的默认配置和自动化机制，帮助开发者更快速地创建生产级别的 Spring 应用程序。Spring Boot 不仅包含了 Spring MVC，还包括了许多其他的 Spring Framework 组件和第三方库。</font>

<font style="background-color:rgb(247, 248, 249);">特点：</font>

<font style="background-color:rgb(247, 248, 249);">内置了一个 Tomcat 服务器，可以轻松地启动和运行应用程序。</font>

<font style="background-color:rgb(247, 248, 249);">通过 starter 依赖项简化了配置和依赖管理，例如 spring-boot-starter-web。</font>

<font style="background-color:rgb(247, 248, 249);">提供了许多自动化配置和约定优于配置的特性，使得开发者可以专注于业务逻辑而不是基础设施。</font>

<font style="background-color:rgb(247, 248, 249);">支持多种开发工具和 IDE，例如 Spring Initializr 和 Spring Tool Suite。</font>

<font style="background-color:rgb(247, 248, 249);">可以生成可执行的 JAR 文件，方便部署和运行。</font>

## <font style="background-color:rgb(247, 248, 249);">区别</font>
1. **<font style="background-color:rgb(247, 248, 249);">范围不同</font>**<font style="background-color:rgb(247, 248, 249);">：Spring MVC 是一个专门的 Web 框架，而 Spring Boot 是一个更广泛的应用程序框架，涵盖了多个领域，包括 Web、数据访问、安全性等。</font>
2. **<font style="background-color:rgb(247, 248, 249);">配置方式不同</font>**<font style="background-color:rgb(247, 248, 249);">：Spring MVC 需要手动配置许多组件和依赖项，而 Spring Boot 提供了默认配置和自动化机制，减少了配置工作。</font>
3. **<font style="background-color:rgb(247, 248, 249);">项目结构不同</font>**<font style="background-color:rgb(247, 248, 249);">：Spring Boot 项目通常有一个更简单的结构，所有的组件和依赖项都集中在一起。</font>
4. **<font style="background-color:rgb(247, 248, 249);">目标不同</font>**<font style="background-color:rgb(247, 248, 249);">：Spring MVC 主要关注于构建 Web 应用程序的核心功能，而 Spring Boot 旨在简化整个应用程序的开发和部署过程。</font>

<font style="background-color:rgb(247, 248, 249);">Spring MVC 是一个专注于 Web 开发的框架，而 Spring Boot 是一个更全面的框架，旨在简化整个应用程序的开发和部署。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/vcoapsdnewq3fp65>