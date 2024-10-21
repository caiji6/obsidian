# 👌如何在 Spring MVC 中实现跨域资源共享（CORS）？

# 题目详细答案
## <font style="background-color:rgb(247, 248, 249);">使用 </font>`<font style="background-color:rgb(247, 248, 249);">@CrossOrigin</font>`<font style="background-color:rgb(247, 248, 249);"> 注解</font>
`<font style="background-color:rgb(247, 248, 249);">@CrossOrigin</font>`<font style="background-color:rgb(247, 248, 249);"> 是 Spring Framework 提供的一个注解，用于在控制器方法或整个控制器上指定允许的跨域请求。这个注解可以在方法级别或类级别使用。</font>

```plain
@RestController
public class MyController {

    @GetMapping("/api/data")
    @CrossOrigin(origins = "http://localhost:3000", maxAge = 3600)
    public List<Data> getData() {
        // 返回数据
    }
}
```

`<font style="background-color:rgb(247, 248, 249);">@CrossOrigin</font>`<font style="background-color:rgb(247, 248, 249);"> 注解指定了来自 </font>`<font style="background-color:rgb(247, 248, 249);">http://localhost:3000</font>`<font style="background-color:rgb(247, 248, 249);"> 的跨域请求是被允许的，并且设置了预检请求的缓存时间为 3600 秒（1 小时）。</font>

```plain
@RestController
@CrossOrigin(origins = "http://localhost:3000", maxAge = 3600)
public class MyController {

    @GetMapping("/api/data")
    public List<Data> getData() {
        // 返回数据
    }
}
```

<font style="background-color:rgb(247, 248, 249);">所有在 </font>`<font style="background-color:rgb(247, 248, 249);">MyController</font>`<font style="background-color:rgb(247, 248, 249);"> 中的方法都将允许来自 </font>`<font style="background-color:rgb(247, 248, 249);">http://localhost:3000</font>`<font style="background-color:rgb(247, 248, 249);"> 的跨域请求。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 </font>`<font style="background-color:rgb(247, 248, 249);">WebMvcConfigurer</font>`<font style="background-color:rgb(247, 248, 249);"> 接口</font>
<font style="background-color:rgb(247, 248, 249);">如果你需要在全局范围内配置 CORS 策略，可以实现</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">WebMvcConfigurer</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接口并重写</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">addCorsMappings</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">方法。</font>

```plain
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
               .allowedOrigins("http://localhost:3000")
               .maxAge(3600);
    }
}
```

<font style="background-color:rgb(247, 248, 249);">创建一个 </font>`<font style="background-color:rgb(247, 248, 249);">CorsConfig</font>`<font style="background-color:rgb(247, 248, 249);"> 类，实现了 </font>`<font style="background-color:rgb(247, 248, 249);">WebMvcConfigurer</font>`<font style="background-color:rgb(247, 248, 249);"> 接口，并在 </font>`<font style="background-color:rgb(247, 248, 249);">addCorsMappings</font>`<font style="background-color:rgb(247, 248, 249);"> 方法中添加了一个全局的 CORS 配置，允许来自 </font>`<font style="background-color:rgb(247, 248, 249);">http://localhost:3000</font>`<font style="background-color:rgb(247, 248, 249);"> 的跨域请求。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/wb90r4dew0m4eori>