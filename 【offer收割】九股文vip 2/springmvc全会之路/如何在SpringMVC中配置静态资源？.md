# 👌如何在 Spring MVC 中配置静态资源？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring MVC 中，配置静态资源可以让应用程序直接提供静态文件（如 HTML、CSS、JavaScript、图片等），而不必通过控制器来处理这些请求。</font>

## <font style="background-color:rgb(247, 248, 249);">将静态资源放置在正确的位置</font>
<font style="background-color:rgb(247, 248, 249);">首先，需要将静态资源文件放置在 Spring MVC 可以访问的目录下。通常，这些文件会被放置在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/static</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">或</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">src/main/webapp</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">目录中。</font>

## <font style="background-color:rgb(247, 248, 249);">在 Spring MVC 配置中启用静态资源处理</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring MVC 的配置文件中（例如</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">spring-mvc.xml</font>`<font style="background-color:rgb(247, 248, 249);">），添加以下配置来启用静态资源处理：</font>

```plain
<mvc:resources mapping="/resources/**" location="/resources/" />
```

<font style="background-color:rgb(247, 248, 249);">这段代码告诉 Spring MVC，将所有以</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">/resources/</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">开头的请求映射到</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/static</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">目录下的文件。</font>

<font style="background-color:rgb(247, 248, 249);">如果你使用 Spring Boot，可以在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">或</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中添加以下配置：</font>

```plain
spring.mvc.static-path-pattern=/resources/**
```

<font style="background-color:rgb(247, 248, 249);">这将启用同样的静态资源处理功能。</font>

## <font style="background-color:rgb(247, 248, 249);">访问静态资源</font>
<font style="background-color:rgb(247, 248, 249);">现在可以通过在浏览器中输入 </font>`<font style="background-color:rgb(247, 248, 249);">http://localhost:8080/resources/your-static-file.html</font>`<font style="background-color:rgb(247, 248, 249);"> 来访问静态资源文件。其中，</font>`<font style="background-color:rgb(247, 248, 249);">your-static-file.html</font>`<font style="background-color:rgb(247, 248, 249);"> 是你放置在 </font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/static</font>`<font style="background-color:rgb(247, 248, 249);"> 目录下的实际文件名。</font>

## <font style="background-color:rgb(247, 248, 249);">自定义静态资源路径</font>
<font style="background-color:rgb(247, 248, 249);">如果你想使用自定义的路径来访问静态资源，可以在配置中修改</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">mapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">location</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性。例如：</font>

```plain
<mvc:resources mapping="/my-resources/**" location="/my-resources/" />
```

<font style="background-color:rgb(247, 248, 249);">这将使得所有以 </font>`<font style="background-color:rgb(247, 248, 249);">/my-resources/</font>`<font style="background-color:rgb(247, 248, 249);"> 开头的请求都被映射到 </font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/static/my-resources</font>`<font style="background-color:rgb(247, 248, 249);"> 目录下的文件。</font>

<font style="background-color:rgb(247, 248, 249);"></font>

<font style="background-color:rgb(247, 248, 249);"></font>

<font style="background-color:rgb(247, 248, 249);"></font>

<font style="background-color:rgb(247, 248, 249);"></font>

<font style="background-color:rgb(247, 248, 249);"></font>

<font style="background-color:rgb(247, 248, 249);"></font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ei2gg6lxc2c7aqw6>