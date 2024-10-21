# 👌什么是 View Resolver？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">View Resolver</font>`<font style="background-color:rgb(247, 248, 249);"> 负责将逻辑视图名称解析为具体的视图对象（如 JSP、Thymeleaf 模板等）。</font>`<font style="background-color:rgb(247, 248, 249);">View Resolver</font>`<font style="background-color:rgb(247, 248, 249);"> 的主要作用是根据控制器返回的视图名称，找到相应的视图资源，并将其渲染成最终的 HTML 响应。</font>

## <font style="background-color:rgb(247, 248, 249);">主要职责</font>
1. **<font style="background-color:rgb(247, 248, 249);">视图名称解析</font>**<font style="background-color:rgb(247, 248, 249);">：将控制器返回的逻辑视图名称解析为具体的视图对象。</font>
2. **<font style="background-color:rgb(247, 248, 249);">视图对象返回</font>**<font style="background-color:rgb(247, 248, 249);">：返回一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">View</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，该对象可以用来渲染模型数据。</font>

## <font style="background-color:rgb(247, 248, 249);">工作流程</font>
1. **<font style="background-color:rgb(247, 248, 249);">控制器处理请求</font>**<font style="background-color:rgb(247, 248, 249);">：当一个 HTTP 请求到达</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">时，它会通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">调用控制器的方法来处理请求。</font>
2. **<font style="background-color:rgb(247, 248, 249);">返回视图名称</font>**<font style="background-color:rgb(247, 248, 249);">：控制器方法处理完请求后，会返回一个包含视图名称和模型数据的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象。</font>
3. **<font style="background-color:rgb(247, 248, 249);">视图名称解析</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">View Resolver</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">将逻辑视图名称解析为具体的视图对象。</font>
4. **<font style="background-color:rgb(247, 248, 249);">渲染视图</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">View</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象使用模型数据来渲染最终的 HTML 响应。</font>

## <font style="background-color:rgb(247, 248, 249);">常见的 </font>`<font style="background-color:rgb(247, 248, 249);">View Resolver</font>`<font style="background-color:rgb(247, 248, 249);"> 实现</font>
1. `**<font style="background-color:rgb(247, 248, 249);">InternalResourceViewResolver</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">最常用的 </font>`<font style="background-color:rgb(247, 248, 249);">View Resolver</font>`<font style="background-color:rgb(247, 248, 249);"> 实现。用于解析 JSP 文件。通过配置前缀和后缀来确定视图的实际路径。</font>

2. `**<font style="background-color:rgb(247, 248, 249);">ThymeleafViewResolver</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用于解析 Thymeleaf 模板文件。需要配合 Thymeleaf 模板引擎使用。</font>

3. `**<font style="background-color:rgb(247, 248, 249);">BeanNameViewResolver</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">通过视图名称作为 bean 名称来查找视图对象。适用于视图对象作为 Spring bean 定义的情况。</font>

4. `**<font style="background-color:rgb(247, 248, 249);">XmlViewResolver</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">通过 XML 文件配置视图名称和视图对象的映射关系。</font>

## <font style="background-color:rgb(247, 248, 249);">代码 Demo</font>
<font style="background-color:rgb(247, 248, 249);">使用 </font>`<font style="background-color:rgb(247, 248, 249);">InternalResourceViewResolver</font>`<font style="background-color:rgb(247, 248, 249);"> ：</font>

```plain
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```

`<font style="background-color:rgb(247, 248, 249);">InternalResourceViewResolver</font>`<font style="background-color:rgb(247, 248, 249);"> 被配置为视图解析器。</font>

<font style="background-color:rgb(247, 248, 249);">视图的前缀被设置为 </font>`<font style="background-color:rgb(247, 248, 249);">/WEB-INF/views/</font>`<font style="background-color:rgb(247, 248, 249);">，后缀被设置为 </font>`<font style="background-color:rgb(247, 248, 249);">.jsp</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

<font style="background-color:rgb(247, 248, 249);">例如，当控制器返回视图名称 </font>`<font style="background-color:rgb(247, 248, 249);">home</font>`<font style="background-color:rgb(247, 248, 249);"> 时，</font>`<font style="background-color:rgb(247, 248, 249);">InternalResourceViewResolver</font>`<font style="background-color:rgb(247, 248, 249);"> 会将其解析为 </font>`<font style="background-color:rgb(247, 248, 249);">/WEB-INF/views/home.jsp</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

## <font style="background-color:rgb(247, 248, 249);">Demo</font>
```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HomeController {

    @GetMapping("/home")
    public ModelAndView home() {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("home");
        modelAndView.addObject("message", "Welcome to the home page!");
        return modelAndView;
    }
}
```

+ `<font style="background-color:rgb(247, 248, 249);">HomeController</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">类定义了一个处理</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">/home</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">请求的方法。</font>
+ <font style="background-color:rgb(247, 248, 249);">该方法返回一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，其中视图名称为</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">home</font>`<font style="background-color:rgb(247, 248, 249);">。</font>
+ `<font style="background-color:rgb(247, 248, 249);">InternalResourceViewResolver</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会将视图名称</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">home</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">解析为</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">/WEB-INF/views/home.jsp</font>`<font style="background-color:rgb(247, 248, 249);">。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/zu6g2e25ix73vyyp>