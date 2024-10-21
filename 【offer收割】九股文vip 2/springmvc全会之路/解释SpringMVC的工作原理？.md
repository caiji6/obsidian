# 👌解释 Spring MVC 的工作原理？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">Spring MVC 的工作原理基于 Model-View-Controller（MVC）设计模式，旨在将应用程序的业务逻辑、用户界面和数据分离开来。</font>

## <font style="background-color:rgb(247, 248, 249);">用户请求</font>
<font style="background-color:rgb(247, 248, 249);">用户通过浏览器发送 HTTP 请求到服务器。例如，用户访问</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">http://example.com/hello</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">前端控制器（DispatcherServlet）</font>
<font style="background-color:rgb(247, 248, 249);">Spring MVC 的核心组件</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">充当前端控制器，它拦截所有进入的 HTTP 请求。</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">web.xml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中配置，负责初始化 Spring MVC 的上下文环境。</font>

```plain
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

## <font style="background-color:rgb(247, 248, 249);">处理器映射（Handler Mapping）</font>
`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接收到请求后，会根据请求 URL 通过处理器映射（Handler Mapping）找到相应的控制器（Controller）。处理器映射是由</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerMapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接口实现的，常见的实现包括</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">RequestMappingHandlerMapping</font>`<font style="background-color:rgb(247, 248, 249);">，它会扫描控制器中的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解。</font>

```plain
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public ModelAndView helloWorld() {
        String message = "Hello, Spring MVC!";
        return new ModelAndView("hello", "message", message);
    }
}
```

## <font style="background-color:rgb(247, 248, 249);">控制器处理</font>
<font style="background-color:rgb(247, 248, 249);">找到相应的控制器后，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">调用控制器的方法处理请求。控制器执行业务逻辑，通常会调用服务层或数据访问层获取数据，并将数据封装到模型中。</font>

```plain
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public ModelAndView helloWorld() {
        String message = "Hello, Spring MVC!";
        return new ModelAndView("hello", "message", message);
    }
}
```

## <font style="background-color:rgb(247, 248, 249);">视图解析器（View Resolver）</font>
<font style="background-color:rgb(247, 248, 249);">控制器处理完请求后，会返回一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，其中包含视图名称和模型数据。</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">使用视图解析器（View Resolver）将视图名称解析为实际的视图对象。常见的视图解析器包括</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">InternalResourceViewResolver</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">ThymeleafViewResolver</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">等。</font>

```plain
@Bean
public InternalResourceViewResolver viewResolver() {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
}
```

## <font style="background-color:rgb(247, 248, 249);">视图渲染</font>
<font style="background-color:rgb(247, 248, 249);">视图解析器将视图名称解析为实际的视图对象后，视图对象负责将模型数据渲染为用户界面，通常是 HTML 页面。视图对象可以是 JSP、Thymeleaf 模板、FreeMarker 模板等。</font>

```plain
<!-- hello.jsp -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<body>
    <h2>${message}</h2>
</body>
</html>
```

## <font style="background-color:rgb(247, 248, 249);">响应返回</font>
<font style="background-color:rgb(247, 248, 249);">渲染后的视图返回给 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 将最终的响应发送回用户浏览器。用户在浏览器中看到渲染后的页面。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ybrrzf4ouct8223n>