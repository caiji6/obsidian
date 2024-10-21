# 👌什么是 Handler Adapter？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);">负责将处理器（Handler）适配为具体的处理方法。</font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> 的主要作用是根据处理器的类型和具体实现，执行相应的处理逻辑。</font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> 是 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 和具体处理器之间的桥梁。</font>

## <font style="background-color:rgb(247, 248, 249);">主要职责</font>
1. **<font style="background-color:rgb(247, 248, 249);">处理器执行</font>**<font style="background-color:rgb(247, 248, 249);">：调用处理器的方法来处理请求。</font>
2. **<font style="background-color:rgb(247, 248, 249);">返回模型和视图</font>**<font style="background-color:rgb(247, 248, 249);">：处理完请求后，返回一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，包含视图名称和模型数据。</font>

## <font style="background-color:rgb(247, 248, 249);">工作流程</font>
1. **<font style="background-color:rgb(247, 248, 249);">请求到达</font>****<font style="background-color:rgb(247, 248, 249);"> </font>**`**<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>**`<font style="background-color:rgb(247, 248, 249);">：当一个 HTTP 请求到达</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">时，它会先通过</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">找到对应的处理器。</font>
2. **<font style="background-color:rgb(247, 248, 249);">选择</font>****<font style="background-color:rgb(247, 248, 249);"> </font>**`**<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">根据处理器的类型选择合适的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);">。</font>
3. **<font style="background-color:rgb(247, 248, 249);">执行处理器</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">调用处理器的方法来处理请求。</font>
4. **<font style="background-color:rgb(247, 248, 249);">返回结果</font>**<font style="background-color:rgb(247, 248, 249);">：处理完请求后，</font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">返回一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">再根据这个对象生成最终的响应。</font>

## <font style="background-color:rgb(247, 248, 249);">常见的 </font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> 实现</font>
1. `**<font style="background-color:rgb(247, 248, 249);">HttpRequestHandlerAdapter</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用于处理实现 </font>`<font style="background-color:rgb(247, 248, 249);">HttpRequestHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 接口的处理器。例如实现了 </font>`<font style="background-color:rgb(247, 248, 249);">HttpRequestHandler</font>`<font style="background-color:rgb(247, 248, 249);"> 接口的处理器。</font>

2. `**<font style="background-color:rgb(247, 248, 249);">SimpleControllerHandlerAdapter</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用于处理实现 </font>`<font style="background-color:rgb(247, 248, 249);">Controller</font>`<font style="background-color:rgb(247, 248, 249);"> 接口的处理器。例如实现了 </font>`<font style="background-color:rgb(247, 248, 249);">Controller</font>`<font style="background-color:rgb(247, 248, 249);"> 接口的处理器。</font>

3. `**<font style="background-color:rgb(247, 248, 249);">RequestMappingHandlerAdapter</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">最常用的 </font>`<font style="background-color:rgb(247, 248, 249);">Handler Adapter</font>`<font style="background-color:rgb(247, 248, 249);"> 实现。用于处理使用 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 注解的控制器方法。支持复杂的请求映射规则和数据绑定。</font>

## <font style="background-color:rgb(247, 248, 249);">代码 Demo</font>
```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Controller
@RequestMapping("/hello")
public class HelloController {

    @GetMapping
    public ModelAndView helloWorld() {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("hello");
        modelAndView.addObject("message", "Hello, World!");
        return modelAndView;
    }
}

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example")
public class WebConfig implements WebMvcConfigurer {
    // 可以在这里添加其他配置
}
```

+ `<font style="background-color:rgb(247, 248, 249);">HelloController</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">类使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@GetMapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解来定义请求映射。</font>
+ `<font style="background-color:rgb(247, 248, 249);">RequestMappingHandlerAdapter</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会根据注解找到</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">helloWorld</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">方法并执行它。</font>
+ `<font style="background-color:rgb(247, 248, 249);">helloWorld</font>`<font style="background-color:rgb(247, 248, 249);"> 方法返回一个 </font>`<font style="background-color:rgb(247, 248, 249);">ModelAndView</font>`<font style="background-color:rgb(247, 248, 249);"> 对象，包含视图名称和模型数据。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hxbmgkqu2tc2uhge>