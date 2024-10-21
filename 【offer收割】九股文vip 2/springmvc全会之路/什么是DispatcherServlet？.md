# 👌什么是 DispatcherServlet？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">充当前端控制器（Front Controller），负责接收所有进入的 HTTP 请求并将它们分派给适当的处理器进行处理。</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 是实现 MVC 模式的关键部分，负责协调整个请求处理流程。</font>

## <font style="background-color:rgb(247, 248, 249);">主要职责</font>
1. **<font style="background-color:rgb(247, 248, 249);">请求接收和分派</font>**<font style="background-color:rgb(247, 248, 249);">：拦截所有进入的 HTTP 请求并将它们分派给适当的控制器（Controller）。</font>
2. **<font style="background-color:rgb(247, 248, 249);">处理器映射</font>**<font style="background-color:rgb(247, 248, 249);">：根据请求 URL，查找相应的处理器（通常是控制器方法）。</font>
3. **<font style="background-color:rgb(247, 248, 249);">视图解析</font>**<font style="background-color:rgb(247, 248, 249);">：将控制器返回的视图名称解析为实际的视图对象。</font>
4. **<font style="background-color:rgb(247, 248, 249);">请求处理</font>**<font style="background-color:rgb(247, 248, 249);">：调用处理器进行请求处理，并将处理结果封装到模型中。</font>
5. **<font style="background-color:rgb(247, 248, 249);">视图渲染</font>**<font style="background-color:rgb(247, 248, 249);">：将模型数据传递给视图对象进行渲染，并生成最终的响应。</font>

## <font style="background-color:rgb(247, 248, 249);">工作流程</font>
1. **<font style="background-color:rgb(247, 248, 249);">初始化</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">在应用程序启动时，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 被初始化。它加载 Spring 应用程序上下文，配置处理器映射、视图解析器等组件。</font>

2. **<font style="background-color:rgb(247, 248, 249);">接收请求</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用户通过浏览器发送 HTTP 请求到服务器。</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 拦截所有符合配置的 URL 模式的请求。</font>

3. **<font style="background-color:rgb(247, 248, 249);">处理器映射</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 使用处理器映射器（Handler Mapping）根据请求 URL 查找相应的处理器（Controller）。</font>

4. **<font style="background-color:rgb(247, 248, 249);">调用处理器</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">找到处理器后，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 调用处理器的方法进行请求处理。处理器执行业务逻辑，通常会调用服务层或数据访问层获取数据，并将数据封装到模型中。</font>

5. **<font style="background-color:rgb(247, 248, 249);">视图解析</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">处理器处理完请求后，返回一个包含视图名称和模型数据的 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 使用视图解析器（View Resolver）将视图名称解析为实际的视图对象。</font>

6. **<font style="background-color:rgb(247, 248, 249);">视图渲染</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">视图对象负责将模型数据渲染为用户界面，通常是 HTML 页面。</font>

7. **<font style="background-color:rgb(247, 248, 249);">响应返回</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">渲染后的视图返回给 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 将最终的响应发送回用户浏览器。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/uoatrwbh0v39mwub>