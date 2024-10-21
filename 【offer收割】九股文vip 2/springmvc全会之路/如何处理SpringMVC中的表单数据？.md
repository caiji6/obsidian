# 👌如何处理 Spring MVC 中的表单数据？

# 题目详细答案
1. **<font style="background-color:rgb(247, 248, 249);">创建表单页面</font>**<font style="background-color:rgb(247, 248, 249);">：使用 HTML 表单元素。</font>
2. **<font style="background-color:rgb(247, 248, 249);">创建数据模型</font>**<font style="background-color:rgb(247, 248, 249);">：定义一个 Java 类来接收表单数据。</font>
3. **<font style="background-color:rgb(247, 248, 249);">创建控制器方法</font>**<font style="background-color:rgb(247, 248, 249);">：处理表单提交请求。</font>
4. **<font style="background-color:rgb(247, 248, 249);">数据绑定和验证</font>**<font style="background-color:rgb(247, 248, 249);">：使用 Spring 提供的验证机制来验证表单数据。</font>

## <font style="background-color:rgb(247, 248, 249);">创建表单页面</font>
<font style="background-color:rgb(247, 248, 249);">首先，创建一个 HTML 表单页面来收集用户输入。一个简单的用户注册表单：</font>

```plain
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
</head>
<body>
    <h2>Register</h2>
    <form action="/register" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username"><br><br>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password"><br><br>
        <input type="submit" value="Register">
    </form>
</body>
</html>
```

## <font style="background-color:rgb(247, 248, 249);">创建数据模型</font>
<font style="background-color:rgb(247, 248, 249);">创建一个 Java 类来表示表单数据。一个用户类：</font>

```plain
public class User {
    private String username;
    private String password;

    // Getters and Setters
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

## <font style="background-color:rgb(247, 248, 249);">创建控制器方法</font>
<font style="background-color:rgb(247, 248, 249);">在控制器类中创建方法来处理表单提交请求。</font>

```plain
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class UserController {

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String submitForm(@ModelAttribute("user") User user, Model model) {
        // 处理表单数据
        model.addAttribute("message", "User registered successfully");
        return "result";
    }
}
```

+ `<font style="background-color:rgb(247, 248, 249);">@GetMapping("/register")</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">方法用于显示注册表单。</font>
+ `<font style="background-color:rgb(247, 248, 249);">@PostMapping("/register")</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">方法用于处理表单提交。通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解，Spring MVC 会自动将表单数据绑定到</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象。</font>

## <font style="background-color:rgb(247, 248, 249);">数据绑定和验证</font>
<font style="background-color:rgb(247, 248, 249);">为了确保表单数据的有效性，可以使用 Spring 的验证机制。首先，在数据模型类上使用注解进行验证：</font>

```plain
import javax.validation.constraints.NotEmpty;

public class User {
    @NotEmpty(message = "Username is required")
    private String username;

    @NotEmpty(message = "Password is required")
    private String password;

    // Getters and Setters
}
```

<font style="background-color:rgb(247, 248, 249);">在控制器方法中添加验证逻辑：</font>

```plain
import org.springframework.validation.BindingResult;
import javax.validation.Valid;

@Controller
public class UserController {

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String submitForm(@Valid @ModelAttribute("user") User user, BindingResult bindingResult, Model model) {
        if (bindingResult.hasErrors()) {
            return "register";
        }
        // 处理表单数据
        model.addAttribute("message", "User registered successfully");
        return "result";
    }
}
```

+ `<font style="background-color:rgb(247, 248, 249);">@Valid</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解用于触发验证。</font>
+ `<font style="background-color:rgb(247, 248, 249);">BindingResult</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">参数用于检查验证结果。如果有验证错误，返回到表单页面并显示错误信息。</font>

## <font style="background-color:rgb(247, 248, 249);">显示验证错误信息</font>
<font style="background-color:rgb(247, 248, 249);">在表单页面上显示验证错误信息：</font>

```plain
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
</head>
<body>
    <h2>Register</h2>
    <form action="/register" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" value="${user.username}"><br>
        <span style="color:red">${#fields.hasErrors('username')} ? ${#fields.errors('username')} : ''</span><br><br>
        
        <label for="password">Password:</label>
        <input type="password" id="password" name="password"><br>
        <span style="color:red">${#fields.hasErrors('password')} ? ${#fields.errors('password')} : ''</span><br><br>
        
        <input type="submit" value="Register">
    </form>
</body>
</html>
```

### 


> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/rhh25v5ym0b8fzl4>