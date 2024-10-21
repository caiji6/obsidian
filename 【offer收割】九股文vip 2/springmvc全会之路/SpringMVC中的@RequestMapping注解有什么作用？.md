# 👌Spring MVC 中的 @RequestMapping 注解有什么作用？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于映射 HTTP 请求到处理器方法（控制器方法）上。它可以应用于类级别和方法级别，用于定义请求 URL 和 HTTP 方法的映射关系。</font>

## <font style="background-color:rgb(247, 248, 249);">主要作用</font>
1. **<font style="background-color:rgb(247, 248, 249);">URL 映射</font>**<font style="background-color:rgb(247, 248, 249);">：将特定的 URL 映射到控制器类或方法上。</font>
2. **<font style="background-color:rgb(247, 248, 249);">HTTP 方法映射</font>**<font style="background-color:rgb(247, 248, 249);">：指定处理请求的 HTTP 方法（如 GET、POST、PUT、DELETE 等）。</font>
3. **<font style="background-color:rgb(247, 248, 249);">请求参数和头信息映射</font>**<font style="background-color:rgb(247, 248, 249);">：可以根据请求参数、头信息等进一步细化映射条件。</font>

## <font style="background-color:rgb(247, 248, 249);">详细用法</font>
### <font style="background-color:rgb(247, 248, 249);">类级别和方法级别的结合</font>
`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">可以同时应用于类级别和方法级别，用于构建更复杂的 URL 映射结构。</font>

```plain
@Controller
@RequestMapping("/api")
public class ApiController {

    @RequestMapping("/users")
    public String getUsers() {
        // 处理 /api/users 请求
        return "users";
    }

    @RequestMapping("/products")
    public String getProducts() {
        // 处理 /api/products 请求
        return "products";
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">指定 HTTP 方法</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">method</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性指定处理请求的 HTTP 方法。</font>

```plain
@RequestMapping(value = "/submit", method = RequestMethod.POST)
public String handleSubmit() {
    // 处理 POST 请求 /submit
    return "submitSuccess";
}
```

### <font style="background-color:rgb(247, 248, 249);">请求参数和头信息</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">params</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">headers</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性进一步细化映射条件。</font>

```plain
@RequestMapping(value = "/filter", params = "type=admin")
public String filterAdmin() {
    // 处理包含参数 type=admin 的请求
    return "adminPage";
}

@RequestMapping(value = "/filter", headers = "User-Agent=Mozilla/5.0")
public String filterByUserAgent() {
    // 处理包含特定 User-Agent 头信息的请求
    return "mozillaPage";
}
```

### <font style="background-color:rgb(247, 248, 249);">消息体和内容类型</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">consumes</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">produces</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性指定请求和响应的内容类型。</font>

```plain
@RequestMapping(value = "/json", method = RequestMethod.POST, consumes = "application/json", produces = "application/json")
@ResponseBody
public ResponseEntity<String> handleJsonRequest(@RequestBody String jsonData) {
    // 处理 JSON 请求并返回 JSON 响应
    return ResponseEntity.ok("{\"status\":\"success\"}");
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/xmeaeuo6dyvzaz7u>