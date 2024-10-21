# 👌如何在 Spring MVC 中使用模板引擎（如 Thymeleaf）？

# 题目详细答案
## <font style="background-color:rgb(247, 248, 249);">添加依赖</font>
<font style="background-color:rgb(247, 248, 249);">在项目的 </font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);"> 文件中添加 Thymeleaf 的依赖项：</font>

```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

<font style="background-color:rgb(247, 248, 249);">如果你不是使用 Spring Boot，可以添加依赖项：</font>

```plain
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring4</artifactId>
</dependency>
```

## <font style="background-color:rgb(247, 248, 249);">配置 Thymeleaf</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring Boot 中，Thymeleaf 的配置通常通过 </font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);"> 或 </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> 文件进行。可以在 </font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);"> 中添加以下配置：</font>

```plain
spring.thymeleaf.cache=false
spring.thymeleaf.mode=HTML5
```

<font style="background-color:rgb(247, 248, 249);">这将禁用 Thymeleaf 的模板缓存，并将模板解析模式设置为 HTML5。</font>

<font style="background-color:rgb(247, 248, 249);">如果你不是使用 Spring Boot，可以在 Spring 配置文件中创建一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ThymeleafViewResolver</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">bean来配置 Thymeleaf：</font>

```plain
<bean id="thymeleafViewResolver" class="org.thymeleaf.spring4.view.ThymeleafViewResolver">
    <property name="templateEngine" ref="templateEngine"/>
    <property name="characterEncoding" value="UTF-8"/>
</bean>

<bean id="templateEngine" class="org.thymeleaf.spring4.SpringTemplateEngine">
    <property name="templateResolver" ref="templateResolver"/>
</bean>

<bean id="templateResolver" class="org.thymeleaf.templateresolver.ClassLoaderTemplateResolver">
    <property name="prefix" value="classpath:templates/"/>
    <property name="suffix" value=".html"/>
    <property name="templateMode" value="HTML5"/>
    <property name="characterEncoding" value="UTF-8"/>
</bean>
```

<font style="background-color:rgb(247, 248, 249);">配置指定了 Thymeleaf 的模板位置、解析模式和字符编码等信息。</font>

## <font style="background-color:rgb(247, 248, 249);">创建模板文件</font>
<font style="background-color:rgb(247, 248, 249);">在项目的 </font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/templates</font>`<font style="background-color:rgb(247, 248, 249);"> 目录下创建一个名为 </font>`<font style="background-color:rgb(247, 248, 249);">index.html</font>`<font style="background-color:rgb(247, 248, 249);"> 的模板文件。这个文件将作为我们的视图模板：</font>

```plain
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Thymeleaf Example</title>
</head>
<body>
    <h1 th:text="${message}">Hello World!</h1>
</body>
</html>
```

<font style="background-color:rgb(247, 248, 249);">使用 Thymeleaf 的语法，</font>`<font style="background-color:rgb(247, 248, 249);">th:text</font>`<font style="background-color:rgb(247, 248, 249);"> 属性将显示传入的 </font>`<font style="background-color:rgb(247, 248, 249);">message</font>`<font style="background-color:rgb(247, 248, 249);"> 变量的值。</font>

## <font style="background-color:rgb(247, 248, 249);">在控制器中使用模板</font>
<font style="background-color:rgb(247, 248, 249);">在控制器中，使用 </font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> 注解和 </font>`<font style="background-color:rgb(247, 248, 249);">@GetMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 注解来处理 HTTP GET 请求，并返回一个 </font>`<font style="background-color:rgb(247, 248, 249);">String</font>`<font style="background-color:rgb(247, 248, 249);"> 对象作为视图名：</font>

```plain
@Controller
public class MyController {

    @GetMapping("/")
    public String index(Model model) {
        model.addAttribute("message", "Welcome to Thymeleaf!");
        return "index";
    }
}
```

<font style="background-color:rgb(247, 248, 249);">我们将一个名为 </font>`<font style="background-color:rgb(247, 248, 249);">message</font>`<font style="background-color:rgb(247, 248, 249);"> 的模型属性添加到模型中，并将其传递给 Thymeleaf 模板。然后，控制器返回 </font>`<font style="background-color:rgb(247, 248, 249);">index</font>`<font style="background-color:rgb(247, 248, 249);"> 视图名，Spring MVC 会自动选择并渲染 </font>`<font style="background-color:rgb(247, 248, 249);">index.html</font>`<font style="background-color:rgb(247, 248, 249);"> 模板文件。当访问应用程序的根路径时，应该能看到一个带有欢迎消息的网页。</font>

<font style="background-color:rgb(247, 248, 249);"></font>

<font style="background-color:rgb(247, 248, 249);"></font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/mnkbudii9i33cbzo>