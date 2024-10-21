# 👌什么是 Spring MVC 的拦截器（Interceptor）？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring MVC 中，拦截器（Interceptor）是一种用于对 HTTP 请求进行预处理和后处理的机制。拦截器可以在请求到达控制器之前、控制器处理请求之后以及视图渲染之前执行特定的逻辑。它们类似于 Servlet 中的过滤器（Filter），但提供了更细粒度的控制和更强大的功能。</font>

## <font style="background-color:rgb(247, 248, 249);">拦截器的主要用途</font>
1. **<font style="background-color:rgb(247, 248, 249);">请求日志记录</font>**<font style="background-color:rgb(247, 248, 249);">：记录每个请求的详细信息，如请求 URL、请求参数、处理时间等。</font>
2. **<font style="background-color:rgb(247, 248, 249);">权限验证</font>**<font style="background-color:rgb(247, 248, 249);">：在请求到达控制器之前检查用户是否有权限访问特定的资源。</font>
3. **<font style="background-color:rgb(247, 248, 249);">性能监控</font>**<font style="background-color:rgb(247, 248, 249);">：测量请求的处理时间，帮助优化性能。</font>
4. **<font style="background-color:rgb(247, 248, 249);">通用处理</font>**<font style="background-color:rgb(247, 248, 249);">：在请求处理之前或之后执行一些通用的逻辑，如设置公共属性、国际化处理等。</font>

## <font style="background-color:rgb(247, 248, 249);">拦截器的生命周期方法</font>
<font style="background-color:rgb(247, 248, 249);">Spring MVC 的拦截器通过实现</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerInterceptor</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接口来定义。这个接口包含三个主要方法：</font>

1. `**<font style="background-color:rgb(247, 248, 249);">preHandle</font>**`<font style="background-color:rgb(247, 248, 249);">：在请求处理之前执行。返回</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">true</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">表示继续处理请求，返回</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">false</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">表示中止请求。</font>
2. `**<font style="background-color:rgb(247, 248, 249);">postHandle</font>**`<font style="background-color:rgb(247, 248, 249);">：在请求处理之后、视图渲染之前执行。可以修改视图模型数据。</font>
3. `**<font style="background-color:rgb(247, 248, 249);">afterCompletion</font>**`<font style="background-color:rgb(247, 248, 249);">：在整个请求完成之后（包括视图渲染之后）执行。通常用于资源清理。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hmqxlpff9pboeo3x>