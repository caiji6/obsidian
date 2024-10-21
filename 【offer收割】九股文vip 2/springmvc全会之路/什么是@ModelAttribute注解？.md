# 👌什么是 @ModelAttribute 注解？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 注解是 Spring MVC 中用于绑定请求参数到模型对象的注解。它可以用于方法参数、方法和控制器类中，以便将请求中的数据绑定到模型对象，并将该对象添加到模型中，以便在视图中使用。</font>

## `<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 的使用场景</font>
1. **<font style="background-color:rgb(247, 248, 249);">方法参数</font>**<font style="background-color:rgb(247, 248, 249);">：用于绑定请求参数到方法参数，并将该参数添加到模型中。</font>
2. **<font style="background-color:rgb(247, 248, 249);">方法</font>**<font style="background-color:rgb(247, 248, 249);">：用于在处理请求之前准备模型数据。通常用于在处理请求之前初始化一些公共数据。</font>
3. **<font style="background-color:rgb(247, 248, 249);">控制器类</font>**<font style="background-color:rgb(247, 248, 249);">：用于在所有请求处理方法之前初始化模型数据。</font>

### <font style="background-color:rgb(247, 248, 249);">用于方法参数</font>
<font style="background-color:rgb(247, 248, 249);">当 </font>`<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于控制器方法参数时，它会自动将请求参数绑定到该参数对象中，并将该对象添加到模型中。</font>

```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class UserController {

    @PostMapping("/register")
    public String registerUser(@ModelAttribute User user) {
        // user 对象已经绑定了请求参数
        // 可以在这里处理业务逻辑
        return "result";
    }
}
```

<font style="background-color:rgb(247, 248, 249);">在</font>`<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于 </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> 对象的参数。这意味着 Spring MVC 会自动将请求参数绑定到 </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> 对象的属性中，并将该对象添加到模型中。</font>

### <font style="background-color:rgb(247, 248, 249);">用于方法</font>
<font style="background-color:rgb(247, 248, 249);">当 </font>`<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于控制器方法时，该方法会在每个处理请求的方法之前执行，用于准备模型数据</font>

```plain
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class UserController {

    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("commonAttribute", "This is a common attribute");
    }

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }
}
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">addAttributes</font>`<font style="background-color:rgb(247, 248, 249);"> 方法会在 </font>`<font style="background-color:rgb(247, 248, 249);">showForm</font>`<font style="background-color:rgb(247, 248, 249);"> 方法之前执行，并将一个公共属性添加到模型中。这样，</font>`<font style="background-color:rgb(247, 248, 249);">commonAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 可以在视图中使用。</font>

### <font style="background-color:rgb(247, 248, 249);">用于控制器类</font>
<font style="background-color:rgb(247, 248, 249);">当 </font>`<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于控制器类时，它会在所有请求处理方法之前执行，用于初始化模型数据。</font>

```plain
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/users")
public class UserController {

    @ModelAttribute
    public void addCommonAttributes(Model model) {
        model.addAttribute("appName", "User Management System");
    }

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String registerUser(@ModelAttribute User user) {
        // user 对象已经绑定了请求参数
        // 可以在这里处理业务逻辑
        return "result";
    }
}
```

`<font style="background-color:rgb(247, 248, 249);">addCommonAttributes</font>`<font style="background-color:rgb(247, 248, 249);"> 方法会在所有请求处理方法之前执行，并将一个公共属性添加到模型中。这样，</font>`<font style="background-color:rgb(247, 248, 249);">appName</font>`<font style="background-color:rgb(247, 248, 249);"> 可以在所有视图中使用。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/zr4iv77bw9k8fggh>