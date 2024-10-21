# 👌什么是 @PathVariable 注解？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">@PathVariable</font>`<font style="background-color:rgb(247, 248, 249);"> 注解是 Spring MVC 中用于将 URL 路径中的变量绑定到处理方法的参数上的注解。它允许你从 URL 路径中提取参数，并将这些参数传递给控制器方法，从而实现更加动态和灵活的 URL 路由。</font>

## `<font style="background-color:rgb(247, 248, 249);">@PathVariable</font>`<font style="background-color:rgb(247, 248, 249);"> 的基本用法</font>
### <font style="background-color:rgb(247, 248, 249);">绑定单个路径变量</font>
<font style="background-color:rgb(247, 248, 249);">可以将 URL 路径中的变量绑定到方法参数上：</font>

```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class UserController {

    @GetMapping("/users/{id}")
    @ResponseBody
    public String getUserById(@PathVariable String id) {
        return "User ID: " + id;
    }
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">@PathVariable</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于将 URL 路径中的 </font>`<font style="background-color:rgb(247, 248, 249);">id</font>`<font style="background-color:rgb(247, 248, 249);"> 变量绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">id</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/users/123</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">id</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">123</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">指定路径变量名称</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">value</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性指定路径变量的名称：</font>

```plain
@GetMapping("/users/{userId}")
@ResponseBody
public String getUserById(@PathVariable("userId") String id) {
    return "User ID: " + id;
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">@PathVariable("userId")</font>`<font style="background-color:rgb(247, 248, 249);"> 表示将 URL 路径中的 </font>`<font style="background-color:rgb(247, 248, 249);">userId</font>`<font style="background-color:rgb(247, 248, 249);"> 变量绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">id</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/users/123</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">id</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">123</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">高级用法</font>
### <font style="background-color:rgb(247, 248, 249);">绑定多个路径变量</font>
<font style="background-color:rgb(247, 248, 249);">可以绑定多个路径变量到方法参数上：</font>

```plain
@GetMapping("/users/{userId}/orders/{orderId}")
@ResponseBody
public String getUserOrder(@PathVariable String userId, @PathVariable String orderId) {
    return "User ID: " + userId + ", Order ID: " + orderId;
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">userId</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">orderId</font>`<font style="background-color:rgb(247, 248, 249);"> 路径变量将分别绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">userId</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">orderId</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/users/123/orders/456</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">userId</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">123</font>`<font style="background-color:rgb(247, 248, 249);">，</font>`<font style="background-color:rgb(247, 248, 249);">orderId</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">456</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">绑定到特定类型</font>
<font style="background-color:rgb(247, 248, 249);">可以将路径变量绑定到特定类型的参数上：</font>

```plain
@GetMapping("/products/{productId}")
@ResponseBody
public String getProductById(@PathVariable int productId) {
    return "Product ID: " + productId;
}
```

`<font style="background-color:rgb(247, 248, 249);">productId</font>`<font style="background-color:rgb(247, 248, 249);"> 路径变量将绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">productId</font>`<font style="background-color:rgb(247, 248, 249);"> 上，并自动转换为 </font>`<font style="background-color:rgb(247, 248, 249);">int</font>`<font style="background-color:rgb(247, 248, 249);"> 类型。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/products/789</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">productId</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">789</font>`<font style="background-color:rgb(247, 248, 249);">。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ophnam8v8locvzt7>