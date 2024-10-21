# 👌什么是 Spring MVC 的生命周期？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">Spring MVC 的生命周期主要分为这几个阶段：</font>

## <font style="background-color:rgb(247, 248, 249);">初始化阶段</font>
<font style="background-color:rgb(247, 248, 249);">Spring 容器初始化，包括加载和解析配置文件、创建 Bean 对象等。对于 Spring MVC 应用，通常需要配置 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">，它是 Spring MVC 的核心组件，负责接收请求、处理请求和生成响应。</font>

## <font style="background-color:rgb(247, 248, 249);">处理阶段</font>
<font style="background-color:rgb(247, 248, 249);">当一个请求到达服务器时，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 会根据请求的 URL 找到对应的处理器（Controller）。涉及：</font>

+ **<font style="background-color:rgb(247, 248, 249);">URL匹配</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会遍历所有的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerMapping</font>`<font style="background-color:rgb(247, 248, 249);">，找到能够处理当前请求的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Handler</font>`<font style="background-color:rgb(247, 248, 249);">（即 Controller）。</font>
+ **<font style="background-color:rgb(247, 248, 249);">解析请求参数</font>**<font style="background-color:rgb(247, 248, 249);">：如果请求中包含参数，Spring MVC 会自动将其绑定到</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RequestParam</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">或</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@PathVariable</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解的方法参数中。</font>
+ **<font style="background-color:rgb(247, 248, 249);">执行业务逻辑</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会调用 Controller 的方法，执行相应的业务逻辑。</font>
+ **<font style="background-color:rgb(247, 248, 249);">视图解析</font>**<font style="background-color:rgb(247, 248, 249);">：如果 Controller 方法返回一个视图名，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会根据视图名找到对应的视图解析器，并将模型数据传递给视图。</font>

## <font style="background-color:rgb(247, 248, 249);">渲染阶段</font>
<font style="background-color:rgb(247, 248, 249);">视图会根据模型数据生成最终的响应内容。Spring MVC 支持多种视图技术，包括 JSP、Thymeleaf、Freemarker 等。</font>

## <font style="background-color:rgb(247, 248, 249);">错误处理阶段</font>
<font style="background-color:rgb(247, 248, 249);">如果在处理阶段或渲染阶段发生了错误，Spring MVC 会跳转到错误处理阶段。在这个阶段，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会查找并调用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@ExceptionHandler</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解的方法来处理异常。</font>

## <font style="background-color:rgb(247, 248, 249);">销毁阶段</font>
<font style="background-color:rgb(247, 248, 249);">当应用程序关闭或者 Spring 容器被销毁时，所有的 Bean 对象也会被销毁。对于 Spring MVC 应用，这意味着</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和所有的 Controller 对象都会被销毁。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/mq0874wzdoghpe2e>