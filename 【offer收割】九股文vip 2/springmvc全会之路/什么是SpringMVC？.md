# 👌什么是 Spring MVC？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">Spring MVC 是 Spring 框架中的一个模块，用于构建基于 Web 的应用程序。它遵循 Model-View-Controller#（MVC）设计模式，将业务逻辑、用户界面和数据分离，以促进代码的可维护性和可扩展性。</font>

<font style="background-color:rgb(247, 248, 249);">主要包含几个概念</font>

## <font style="background-color:rgb(247, 248, 249);">模型（Model）</font>
<font style="background-color:rgb(247, 248, 249);">模型代表应用程序的数据和业务逻辑。它通常包含数据对象（如 POJO）和服务层（如 Spring 服务）来处理业务逻辑。模型负责从数据库或其他数据源获取数据，并将数据传递给视图以显示给用户。</font>

## <font style="background-color:rgb(247, 248, 249);">视图（View）</font>
<font style="background-color:rgb(247, 248, 249);">视图负责展示数据，通常是 HTML 页面或其他类型的用户界面。Spring MVC 支持多种视图技术，包括 JSP、Thymeleaf、FreeMarker 等。视图从模型获取数据并将其呈现给用户。</font>

## <font style="background-color:rgb(247, 248, 249);">控制器（Controller）</font>
<font style="background-color:rgb(247, 248, 249);">控制器处理用户请求并决定将数据传递给哪个视图。它接收用户输入，调用模型进行处理，并选择合适的视图来显示结果。控制器通常使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解来标识，并使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解来映射 URL 请求。</font>

## <font style="background-color:rgb(247, 248, 249);">Spring MVC 的工作流程</font>
1. **<font style="background-color:rgb(247, 248, 249);">用户请求</font>**<font style="background-color:rgb(247, 248, 249);">：用户通过浏览器发送 HTTP 请求到服务器。</font>
2. **<font style="background-color:rgb(247, 248, 249);">前端控制器（DispatcherServlet）</font>**<font style="background-color:rgb(247, 248, 249);">：Spring MVC 的前端控制器</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">拦截所有请求并进行分发。</font>
3. **<font style="background-color:rgb(247, 248, 249);">处理器映射（Handler Mapping）</font>**<font style="background-color:rgb(247, 248, 249);">：根据请求 URL，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">查找相应的控制器。</font>
4. **<font style="background-color:rgb(247, 248, 249);">控制器处理</font>**<font style="background-color:rgb(247, 248, 249);">：控制器处理请求，调用服务层或数据访问层以获取数据，并将数据封装到模型中。</font>
5. **<font style="background-color:rgb(247, 248, 249);">视图解析器（View Resolver）</font>**<font style="background-color:rgb(247, 248, 249);">：控制器返回视图名称，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">使用视图解析器将视图名称解析为实际的视图对象。</font>
6. **<font style="background-color:rgb(247, 248, 249);">视图渲染</font>**<font style="background-color:rgb(247, 248, 249);">：视图对象负责将模型数据渲染为用户界面，通常是 HTML 页面。</font>
7. **<font style="background-color:rgb(247, 248, 249);">响应返回</font>**<font style="background-color:rgb(247, 248, 249);">：渲染后的视图返回给</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">将最终的响应发送回用户浏览器。</font>

## <font style="background-color:rgb(247, 248, 249);">核心组件</font>
1. **<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>**<font style="background-color:rgb(247, 248, 249);">：前端控制器，负责接收并分发请求。</font>
2. **<font style="background-color:rgb(247, 248, 249);">Controller</font>**<font style="background-color:rgb(247, 248, 249);">：处理用户请求，包含业务逻辑。</font>
3. **<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>**<font style="background-color:rgb(247, 248, 249);">：包含模型数据和视图名称的对象。</font>
4. **<font style="background-color:rgb(247, 248, 249);">View Resolver</font>**<font style="background-color:rgb(247, 248, 249);">：将视图名称解析为实际的视图对象。</font>
5. **<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>**<font style="background-color:rgb(247, 248, 249);">：根据请求 URL 查找相应的控制器。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/md6axgfq3kdfcoq9>