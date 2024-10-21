# 👌什么是 ModelAndView？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);">用于封装模型数据和视图信息。它允许控制器方法返回一个对象，该对象包含视图名称和模型数据，从而将数据传递给视图进行渲染。</font>

## <font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 的组成部分</font>
1. **<font style="background-color:rgb(247, 248, 249);">视图名称</font>**<font style="background-color:rgb(247, 248, 249);">：表示要渲染的视图的名称，通常对应于某个 JSP、Thymeleaf 模板或其他视图模板。</font>
2. **<font style="background-color:rgb(247, 248, 249);">模型数据</font>**<font style="background-color:rgb(247, 248, 249);">：一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Map</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">或者</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Model</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，包含要传递给视图的数据。</font>



```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class MyController {

    @GetMapping("/welcome")
    public ModelAndView welcome() {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("welcome"); // 设置视图名称
        modelAndView.addObject("message", "Welcome to Spring MVC!"); // 添加模型数据

        return modelAndView;
    }
}
```

`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象被创建。</font>

`<font style="background-color:rgb(247, 248, 249);">setViewName("welcome")</font>`<font style="background-color:rgb(247, 248, 249);"> 设置视图名称为 </font>`<font style="background-color:rgb(247, 248, 249);">welcome</font>`<font style="background-color:rgb(247, 248, 249);">，表示将使用名为 </font>`<font style="background-color:rgb(247, 248, 249);">welcome</font>`<font style="background-color:rgb(247, 248, 249);"> 的视图模板来渲染响应。</font>

`<font style="background-color:rgb(247, 248, 249);">addObject("message", "Welcome to Spring MVC!")</font>`<font style="background-color:rgb(247, 248, 249);"> 添加模型数据，键为 </font>`<font style="background-color:rgb(247, 248, 249);">message</font>`<font style="background-color:rgb(247, 248, 249);">，值为 </font>`<font style="background-color:rgb(247, 248, 249);">"Welcome to Spring MVC!"</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">在视图模板中使用模型数据（假设使用 Thymeleaf）</font>
```plain
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Welcome</title>
</head>
<body>
    <h1 th:text="${message}">Welcome Message</h1>
</body>
</html>
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">${message}</font>`<font style="background-color:rgb(247, 248, 249);"> 表达式将被替换为模型数据中 </font>`<font style="background-color:rgb(247, 248, 249);">message</font>`<font style="background-color:rgb(247, 248, 249);"> 键对应的值。</font>

## <font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 的常用方法</font>
**<font style="background-color:rgb(247, 248, 249);">构造函数</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView()</font>`<font style="background-color:rgb(247, 248, 249);">：创建一个空的 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView(String viewName)</font>`<font style="background-color:rgb(247, 248, 249);">：创建一个带有视图名称的 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView(String viewName, String modelName, Object modelObject)</font>`<font style="background-color:rgb(247, 248, 249);">：创建一个带有视图名称和单个模型数据的 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView(String viewName, Map<String, ?> model)</font>`<font style="background-color:rgb(247, 248, 249);">：创建一个带有视图名称和模型数据的 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>

**<font style="background-color:rgb(247, 248, 249);">设置视图名称</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

`<font style="background-color:rgb(247, 248, 249);">void setViewName(String viewName)</font>`<font style="background-color:rgb(247, 248, 249);">：设置视图名称。</font>

**<font style="background-color:rgb(247, 248, 249);">添加模型数据</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView addObject(String attributeName, Object attributeValue)</font>`<font style="background-color:rgb(247, 248, 249);">：添加单个模型数据。</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView addObject(Object attributeValue)</font>`<font style="background-color:rgb(247, 248, 249);">：添加单个模型数据，属性名为对象的类名。</font>

`<font style="background-color:rgb(247, 248, 249);">ModelAndView addAllObjects(Map<String, ?> modelMap)</font>`<font style="background-color:rgb(247, 248, 249);">：添加多个模型数据。</font>

**<font style="background-color:rgb(247, 248, 249);">获取模型数据</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

`<font style="background-color:rgb(247, 248, 249);">Map<String, Object> getModel()</font>`<font style="background-color:rgb(247, 248, 249);">：获取模型数据。</font>

**<font style="background-color:rgb(247, 248, 249);">获取视图名称</font>**<font style="background-color:rgb(247, 248, 249);">：</font>

`<font style="background-color:rgb(247, 248, 249);">String getViewName()</font>`<font style="background-color:rgb(247, 248, 249);">：获取视图名称。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 的优点</font>
1. **<font style="background-color:rgb(247, 248, 249);">清晰分离模型和视图</font>**<font style="background-color:rgb(247, 248, 249);">：将模型数据和视图信息封装在一个对象中，使得控制器方法的返回值更加清晰和结构化。</font>
2. **<font style="background-color:rgb(247, 248, 249);">灵活性</font>**<font style="background-color:rgb(247, 248, 249);">：可以在一个地方设置视图和模型数据，便于维护和修改。</font>
3. **<font style="background-color:rgb(247, 248, 249);">简化代码</font>**<font style="background-color:rgb(247, 248, 249);">：通过返回</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，可以避免在控制器方法中显式设置模型和视图。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/mc0pglnvpsff65ku>