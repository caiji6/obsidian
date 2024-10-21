# 👌什么是 @RequestParam 注解？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">@RequestParam</font>`<font style="background-color:rgb(247, 248, 249);"> 注解是 Spring MVC 中用于将请求参数绑定到处理方法的参数上的注解。它可以用于从 URL 查询参数、表单数据或其他请求参数中提取值，并将这些值传递给控制器方法的参数。</font>

## `<font style="background-color:rgb(247, 248, 249);">@RequestParam</font>`<font style="background-color:rgb(247, 248, 249);"> 的基本用法</font>
### <font style="background-color:rgb(247, 248, 249);">绑定单个请求参数</font>
```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class UserController {

    @GetMapping("/greet")
    @ResponseBody
    public String greetUser(@RequestParam String name) {
        return "Hello, " + name;
    }
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">@RequestParam</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于将请求参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 的值绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/greet?name=John</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">John</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">指定请求参数名称</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">value</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性指定请求参数的名称：</font>

```plain
@GetMapping("/greet")
@ResponseBody
public String greetUser(@RequestParam("username") String name) {
    return "Hello, " + name;
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">@RequestParam("username")</font>`<font style="background-color:rgb(247, 248, 249);"> 表示将请求参数 </font>`<font style="background-color:rgb(247, 248, 249);">username</font>`<font style="background-color:rgb(247, 248, 249);"> 的值绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/greet?username=John</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">John</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">设置请求参数的默认值</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">defaultValue</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性设置请求参数的默认值：</font>

```plain
@GetMapping("/greet")
@ResponseBody
public String greetUser(@RequestParam(value = "name", defaultValue = "Guest") String name) {
    return "Hello, " + name;
}
```

<font style="background-color:rgb(247, 248, 249);">如果请求 URL 中没有 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 参数，那么 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 参数的默认值将是 </font>`<font style="background-color:rgb(247, 248, 249);">Guest</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">请求参数为可选</font>
<font style="background-color:rgb(247, 248, 249);">可以通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">required</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">属性指定请求参数是否是必需的：</font>

```plain
@GetMapping("/greet")
@ResponseBody
public String greetUser(@RequestParam(value = "name", required = false) String name) {
    if (name == null) {
        name = "Guest";
    }
    return "Hello, " + name;
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">@RequestParam(value = "name", required = false)</font>`<font style="background-color:rgb(247, 248, 249);"> 表示 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 参数是可选的。如果请求 URL 中没有 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 参数，那么 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 参数的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">null</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">高级用法</font>
### <font style="background-color:rgb(247, 248, 249);">绑定多个请求参数</font>
```plain
@GetMapping("/user")
@ResponseBody
public String getUserInfo(@RequestParam String name, @RequestParam int age) {
    return "User: " + name + ", Age: " + age;
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">age</font>`<font style="background-color:rgb(247, 248, 249);"> 请求参数将分别绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">age</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/user?name=John&age=30</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">name</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">John</font>`<font style="background-color:rgb(247, 248, 249);">，</font>`<font style="background-color:rgb(247, 248, 249);">age</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">30</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">绑定到集合类型</font>
```plain
@GetMapping("/numbers")
@ResponseBody
public String getNumbers(@RequestParam List<Integer> nums) {
    return "Numbers: " + nums;
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">nums</font>`<font style="background-color:rgb(247, 248, 249);"> 请求参数将绑定到方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">nums</font>`<font style="background-color:rgb(247, 248, 249);"> 上。如果请求 URL 是 </font>`<font style="background-color:rgb(247, 248, 249);">/numbers?nums=1&nums=2&nums=3</font>`<font style="background-color:rgb(247, 248, 249);">，那么方法参数 </font>`<font style="background-color:rgb(247, 248, 249);">nums</font>`<font style="background-color:rgb(247, 248, 249);"> 的值将是 </font>`<font style="background-color:rgb(247, 248, 249);">[1, 2, 3]</font>`<font style="background-color:rgb(247, 248, 249);">。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/efsy7f24elbmp1gk>