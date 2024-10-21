# 👌如何在 Spring MVC 中使@ExceptionHandler 注解？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring MVC 中，</font>`<font style="background-color:rgb(247, 248, 249);">@ExceptionHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于定义全局异常处理器。它允许你在一个地方集中处理应用程序中的异常，而不是在每个控制器方法中都编写重复的代码。</font>

## <font style="background-color:rgb(247, 248, 249);">创建一个异常处理器方法</font>
<font style="background-color:rgb(247, 248, 249);">在你的控制器类中，创建一个带有 </font>`<font style="background-color:rgb(247, 248, 249);">@ExceptionHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 注解的方法。这个方法将被用来捕获和处理特定类型的异常</font>

```plain
@Controller
public class MyController {

    //...

    @ExceptionHandler(Exception.class)
    public String handleException(Exception ex) {
        // 处理异常并返回视图名或重定向到其他页面
        return "error";
    }
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">handleException()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法将被用来处理所有类型的 </font>`<font style="background-color:rgb(247, 248, 249);">Exception</font>`<font style="background-color:rgb(247, 248, 249);">。如果在控制器方法中抛出一个异常，Spring MVC 将调用这个方法来处理它。</font>

## <font style="background-color:rgb(247, 248, 249);">指定要处理的异常类型</font>
<font style="background-color:rgb(247, 248, 249);">你可以在 </font>`<font style="background-color:rgb(247, 248, 249);">@ExceptionHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 注解中指定要处理的异常类型。</font>

```plain
@ExceptionHandler(value = {SQLException.class, IOException.class})
public String handleDatabaseAndIOErrors(Exception ex) {
    // 处理数据库和 I/O 错误
    return "databaseAndIOError";
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">handleDatabaseAndIOErrors()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法将被用来处理 </font>`<font style="background-color:rgb(247, 248, 249);">SQLException</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">IOException</font>`<font style="background-color:rgb(247, 248, 249);"> 异常。</font>

## <font style="background-color:rgb(247, 248, 249);">设置 HTTP 状态码</font>
<font style="background-color:rgb(247, 248, 249);">通过使用 </font>`<font style="background-color:rgb(247, 248, 249);">@ResponseStatus</font>`<font style="background-color:rgb(247, 248, 249);"> 注解，你可以设置 HTTP 响应的状态码。</font>

```plain
@ExceptionHandler(value = {SQLException.class, IOException.class})
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
public String handleDatabaseAndIOErrors(Exception ex) {
    // 处理数据库和 I/O 错误
    return "databaseAndIOError";
}
```

<font style="background-color:rgb(247, 248, 249);">在</font>`<font style="background-color:rgb(247, 248, 249);">handleDatabaseAndIOErrors()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法将设置 HTTP 响应的状态码为 </font>`<font style="background-color:rgb(247, 248, 249);">500 Internal Server Error</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">访问异常信息</font>
<font style="background-color:rgb(247, 248, 249);">在异常处理器方法中，你可以访问抛出的异常对象并获取更多的信息。</font>

```plain
@ExceptionHandler(value = {SQLException.class, IOException.class})
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
public String handleDatabaseAndIOErrors(Exception ex) {
    // 记录错误信息
    logger.error("An error occurred", ex);
    // 处理数据库和 I/O 错误
    return "databaseAndIOError";
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">logger.error()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法将记录异常信息。</font>

## <font style="background-color:rgb(247, 248, 249);">返回视图或重定向到其他页面</font>
<font style="background-color:rgb(247, 248, 249);">在异常处理器方法中，你可以返回视图名或重定向到其他页面。</font>

```plain
@ExceptionHandler(value = {SQLException.class, IOException.class})
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
public String handleDatabaseAndIOErrors(Exception ex) {
    // 记录错误信息
    logger.error("An error occurred", ex);
    // 返回错误视图
    return "error";
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">handleDatabaseAndIOErrors()</font>`<font style="background-color:rgb(247, 248, 249);"> 方法将返回一个名为 </font>`<font style="background-color:rgb(247, 248, 249);">error</font>`<font style="background-color:rgb(247, 248, 249);"> 的视图。</font>

<font style="background-color:rgb(247, 248, 249);">使用 </font>`<font style="background-color:rgb(247, 248, 249);">@ExceptionHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 注解可以使你的应用程序更加健壮和易于维护，通过集中处理异常来提高代码的可读性和可维护性。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ws4fntl3izs3uslp>