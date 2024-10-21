# 👌如何在 Spring MVC 中使用 @ResponseBody 注解？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于标记方法的返回值将被写入到 HTTP 响应体中。这个注解通常与 </font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> 或 </font>`<font style="background-color:rgb(247, 248, 249);">@RestController</font>`<font style="background-color:rgb(247, 248, 249);"> 结合使用，用于处理 AJAX 请求或返回 JSON、XML 等数据格式的 API 响应。</font>

## <font style="background-color:rgb(247, 248, 249);">添加必要的依赖</font>
<font style="background-color:rgb(247, 248, 249);">确保你的项目中已经添加了 Spring MVC 的依赖。如果你使用 Maven，可以在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中添加以下依赖：</font>

```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

<font style="background-color:rgb(247, 248, 249);">如果你不使用 Spring Boot，也可以单独添加 Spring MVC 的依赖。</font>

## <font style="background-color:rgb(247, 248, 249);">创建一个控制器</font>
<font style="background-color:rgb(247, 248, 249);">创建一个带有 </font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> 或 </font>`<font style="background-color:rgb(247, 248, 249);">@RestController</font>`<font style="background-color:rgb(247, 248, 249);"> 注解的类，表示这是一个 Spring MVC 控制器。</font>

```plain
@RestController
public class MyController {
    
}
```

`<font style="background-color:rgb(247, 248, 249);">@RestController</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">是一个组合注解，相当于同时使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">在方法上使用 @ResponseBody</font>
<font style="background-color:rgb(247, 248, 249);">在控制器的方法上使用 </font>`<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>`<font style="background-color:rgb(247, 248, 249);"> 注解，指示 Spring MVC 将该方法的返回值作为 HTTP 响应体的内容。</font>

```plain
@RestController
public class MyController {

    @GetMapping("/api/data")
    @ResponseBody
    public String getData() {
        return "Hello, World!";
    }
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">getData()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法的返回值 </font>`<font style="background-color:rgb(247, 248, 249);">"Hello, World!"</font>`<font style="background-color:rgb(247, 248, 249);"> 将被写入到 HTTP 响应体中。</font>

## <font style="background-color:rgb(247, 248, 249);">使用对象作为返回值</font>
<font style="background-color:rgb(247, 248, 249);">如果你想要返回一个对象，例如一个 JSON 对象，你可以使用 Jackson 或 Gson 等库来将对象序列化为 JSON 格式。</font>

```plain
@RestController
public class MyController {

    @GetMapping("/api/user")
    @ResponseBody
    public User getUser() {
        User user = new User("John Doe", 30);
        return user;
    }
}

class User {
    private String name;
    private int age;

    // getters and setters
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">getUser()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法返回一个 </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。Spring MVC 会自动将其序列化为 JSON 格式并写入到 HTTP 响应体中。</font>

## <font style="background-color:rgb(247, 248, 249);">处理异常</font>
<font style="background-color:rgb(247, 248, 249);">如果你的方法抛出一个异常，你可以使用 </font>`<font style="background-color:rgb(247, 248, 249);">@ExceptionHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 注解来捕获并处理该异常。</font>

```plain
@RestController
public class MyController {

    @GetMapping("/api/data")
    @ResponseBody
    public String getData() {
        throw new RuntimeException("Something went wrong");
    }

    @ExceptionHandler(RuntimeException.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ResponseBody
    public String handleException(RuntimeException ex) {
        return "An error occurred: " + ex.getMessage();
    }
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">getData()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法抛出一个 </font>`<font style="background-color:rgb(247, 248, 249);">RuntimeException</font>`<font style="background-color:rgb(247, 248, 249);">。</font>`<font style="background-color:rgb(247, 248, 249);">handleException()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法被用来捕获并处理该异常，并返回一个错误消息作为 HTTP 响应体的内容。</font>

<font style="background-color:rgb(247, 248, 249);">总结来说，使用 </font>`<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>`<font style="background-color:rgb(247, 248, 249);"> 注解可以让你的控制器方法的返回值直接写入到 HTTP 响应体中，非常适合用于处理 AJAX 请求或提供 RESTful API。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/mlmq11ocz98xg2gm>