# 👌如何在 Spring MVC 中进行表单验证？

# 题目详细答案
1. **<font style="background-color:rgb(247, 248, 249);">定义数据模型并添加验证注解</font>**<font style="background-color:rgb(247, 248, 249);">。</font>
2. **<font style="background-color:rgb(247, 248, 249);">在控制器中处理表单提交并进行验证</font>**<font style="background-color:rgb(247, 248, 249);">。</font>
3. **<font style="background-color:rgb(247, 248, 249);">在视图中显示验证错误信息</font>**<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">定义数据模型并添加验证注解</font>
<font style="background-color:rgb(247, 248, 249);">使用 Java Bean Validation (JSR-380) 注解来定义数据模型的验证规则。例如，创建一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">类，并在其字段上添加验证注解：</font>

```plain
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

public class User {
    @NotEmpty(message = "Username is required")
    @Size(min = 3, max = 20, message = "Username must be between 3 and 20 characters")
    private String username;

    @NotEmpty(message = "Password is required")
    @Size(min = 6, message = "Password must be at least 6 characters")
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

## <font style="background-color:rgb(247, 248, 249);">在控制器中处理表单提交并进行验证</font>
<font style="background-color:rgb(247, 248, 249);">在控制器中使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@Valid</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">BindingResult</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">参数来处理表单验证。</font>

```plain
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

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

`<font style="background-color:rgb(247, 248, 249);">@Valid</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于触发验证。</font>

`<font style="background-color:rgb(247, 248, 249);">BindingResult</font>`<font style="background-color:rgb(247, 248, 249);"> 参数用于检查验证结果。如果有验证错误，将返回到表单页面并显示错误信息。</font>

## <font style="background-color:rgb(247, 248, 249);">在视图中显示验证错误信息</font>
<font style="background-color:rgb(247, 248, 249);">在视图模板中显示验证错误信息。例如，使用 Thymeleaf 模板引擎：</font>

```plain
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Register</title>
</head>
<body>
    <h2>Register</h2>
    <form action="#" th:action="@{/register}" th:object="${user}" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" th:field="*{username}">
        <div th:if="${#fields.hasErrors('username')}" th:errors="*{username}">Username Error</div>
        <br><br>
        
        <label for="password">Password:</label>
        <input type="password" id="password" th:field="*{password}">
        <div th:if="${#fields.hasErrors('password')}" th:errors="*{password}">Password Error</div>
        <br><br>
        
        <input type="submit" value="Register">
    </form>
</body>
</html>
```

`<font style="background-color:rgb(247, 248, 249);">th:object="${user}"</font>`<font style="background-color:rgb(247, 248, 249);"> 绑定表单对象。</font>

`<font style="background-color:rgb(247, 248, 249);">th:field="*{username}"</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">th:field="*{password}"</font>`<font style="background-color:rgb(247, 248, 249);"> 绑定表单字段。</font>

`<font style="background-color:rgb(247, 248, 249);">th:if="${#fields.hasErrors('username')}"</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">th:errors="*{username}"</font>`<font style="background-color:rgb(247, 248, 249);"> 用于显示验证错误信息。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ts53gcl4wgzdau7h>