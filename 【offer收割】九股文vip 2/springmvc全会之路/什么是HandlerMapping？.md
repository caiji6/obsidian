# 👌什么是 Handler Mapping？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> 负责将 HTTP 请求映射到相应的处理器（通常是控制器方法）。当 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 接收到一个请求时，它会使用 </font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> 来确定哪个处理器应该处理这个请求。</font>

## <font style="background-color:rgb(247, 248, 249);">主要职责</font>
1. **<font style="background-color:rgb(247, 248, 249);">请求映射</font>**<font style="background-color:rgb(247, 248, 249);">：根据请求的 URL、HTTP 方法、请求参数等信息，查找并确定相应的处理器。</font>
2. **<font style="background-color:rgb(247, 248, 249);">处理器返回</font>**<font style="background-color:rgb(247, 248, 249);">：返回一个包含处理器对象和处理器拦截器链的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerExecutionChain</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象。</font>

## <font style="background-color:rgb(247, 248, 249);">工作流程</font>
1. **<font style="background-color:rgb(247, 248, 249);">请求到达</font>****<font style="background-color:rgb(247, 248, 249);"> </font>**`**<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>**`<font style="background-color:rgb(247, 248, 249);">：当一个 HTTP 请求到达</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">时，它会首先交给</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">进行处理。</font>
2. **<font style="background-color:rgb(247, 248, 249);">查找处理器</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">根据请求的 URL、HTTP 方法等信息查找匹配的处理器。</font>
3. **<font style="background-color:rgb(247, 248, 249);">返回处理器</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">返回一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerExecutionChain</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象，其中包含处理器（通常是控制器方法）和处理器拦截器链。</font>
4. **<font style="background-color:rgb(247, 248, 249);">处理请求</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">使用找到的处理器来处理请求，并生成响应。</font>

## <font style="background-color:rgb(247, 248, 249);">常见的 </font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> 实现</font>
1. `**<font style="background-color:rgb(247, 248, 249);">BeanNameUrlHandlerMapping</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">通过 bean 的名称来映射处理器。例如，bean 名称为 </font>`<font style="background-color:rgb(247, 248, 249);">/hello</font>`<font style="background-color:rgb(247, 248, 249);"> 的处理器会处理 </font>`<font style="background-color:rgb(247, 248, 249);">/hello</font>`<font style="background-color:rgb(247, 248, 249);"> 请求。</font>

2. `**<font style="background-color:rgb(247, 248, 249);">SimpleUrlHandlerMapping</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">通过显式配置的 URL 路径来映射处理器。可以在 Spring 配置文件中指定 URL 到处理器的映射关系。</font>

3. `**<font style="background-color:rgb(247, 248, 249);">DefaultAnnotationHandlerMapping</font>**`<font style="background-color:rgb(247, 248, 249);">（过时）：</font>

<font style="background-color:rgb(247, 248, 249);">通过注解（如 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);">）来映射处理器。在较新的 Spring 版本中被 </font>`<font style="background-color:rgb(247, 248, 249);">RequestMappingHandlerMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 取代。</font>

4. `**<font style="background-color:rgb(247, 248, 249);">RequestMappingHandlerMapping</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">这是最常用的 </font>`<font style="background-color:rgb(247, 248, 249);">Handler Mapping</font>`<font style="background-color:rgb(247, 248, 249);"> 实现。通过注解（如 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">@GetMapping</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">@PostMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 等）来映射处理器。支持复杂的请求映射规则，包括路径变量、请求参数、请求头等。</font>

## <font style="background-color:rgb(247, 248, 249);">代码 Demo</font>
```plain
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Controller
@RequestMapping("/hello")
public class HelloController {

    @GetMapping
    public String helloWorld() {
        return "hello";
    }
}

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example")
public class WebConfig implements WebMvcConfigurer {
    // 可以在这里添加其他配置
}
```

`<font style="background-color:rgb(247, 248, 249);">HelloController</font>`<font style="background-color:rgb(247, 248, 249);"> 类使用 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 和 </font>`<font style="background-color:rgb(247, 248, 249);">@GetMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 注解来定义请求映射。</font>

`<font style="background-color:rgb(247, 248, 249);">/hello</font>`<font style="background-color:rgb(247, 248, 249);"> URL 会被映射到 </font>`<font style="background-color:rgb(247, 248, 249);">helloWorld</font>`<font style="background-color:rgb(247, 248, 249);"> 方法。</font>

### 


> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fftsyf1v1gmkng7m>