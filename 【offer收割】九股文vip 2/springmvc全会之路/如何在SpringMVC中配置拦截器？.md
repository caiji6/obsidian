# 👌如何在 Spring MVC 中配置拦截器？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring MVC 中配置拦截器有两种常见方式：使用 Java 配置类和使用 XML 配置文件。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 Java 配置类</font>
<font style="background-color:rgb(247, 248, 249);">通过 Java 配置类来配置拦截器是一种更现代和推荐的方式。需要实现 </font>`<font style="background-color:rgb(247, 248, 249);">WebMvcConfigurer</font>`<font style="background-color:rgb(247, 248, 249);"> 接口，并在其中注册你的拦截器。</font>

1. **<font style="background-color:rgb(247, 248, 249);">创建一个拦截器类</font>**<font style="background-color:rgb(247, 248, 249);">：实现</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerInterceptor</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接口。</font>
2. **<font style="background-color:rgb(247, 248, 249);">创建一个配置类</font>**<font style="background-color:rgb(247, 248, 249);">：实现 </font>`<font style="background-color:rgb(247, 248, 249);">WebMvcConfigurer</font>`<font style="background-color:rgb(247, 248, 249);"> 接口，并在其中注册拦截器。</font>

**<font style="background-color:rgb(247, 248, 249);">拦截器类：</font>**

```plain
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("Pre Handle method is Calling");
        return true; // 返回 true 继续处理请求，返回 false 中止请求
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("Post Handle method is Calling");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception exception) throws Exception {
        System.out.println("Request and Response is completed");
    }
}
```

**<font style="background-color:rgb(247, 248, 249);">配置类：</font>**

```plain
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private MyInterceptor myInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor).addPathPatterns("/**"); // 拦截所有请求
    }
}
```

## <font style="background-color:rgb(247, 248, 249);">使用 XML 配置文件</font>
<font style="background-color:rgb(247, 248, 249);">如果你使用的是 XML 配置文件，可以通过 </font>`<font style="background-color:rgb(247, 248, 249);"><mvc:interceptors></font>`<font style="background-color:rgb(247, 248, 249);"> 元素来配置拦截器。</font>

1. **<font style="background-color:rgb(247, 248, 249);">创建一个拦截器类</font>**<font style="background-color:rgb(247, 248, 249);">：实现</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">HandlerInterceptor</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接口。</font>
2. **<font style="background-color:rgb(247, 248, 249);">在 XML 配置文件中注册拦截器</font>**<font style="background-color:rgb(247, 248, 249);">。</font>

**<font style="background-color:rgb(247, 248, 249);">拦截器类：</font>**

```plain
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("Pre Handle method is Calling");
        return true; // 返回 true 继续处理请求，返回 false 中止请求
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("Post Handle method is Calling");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception exception) throws Exception {
        System.out.println("Request and Response is completed");
    }
}
```

**<font style="background-color:rgb(247, 248, 249);">XML 配置文件：</font>**

```plain
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/mvc
                           http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.example.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>

</beans>
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/oxdbbm16kfnkql8w>