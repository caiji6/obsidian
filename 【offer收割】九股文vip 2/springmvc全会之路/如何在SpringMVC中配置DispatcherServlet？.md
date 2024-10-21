# 👌如何在 Spring MVC 中配置DispatcherServlet？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">配置 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 通常有两种方式：基于 XML 配置和基于 Java 配置（也称为 Java Config 或 Java-based Configuration）。</font>

## <font style="background-color:rgb(247, 248, 249);">基于 XML 配置</font>
### <font style="background-color:rgb(247, 248, 249);">web.xml 配置</font>
<font style="background-color:rgb(247, 248, 249);">在传统的 Spring MVC 应用中，</font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 通常是在 </font>`<font style="background-color:rgb(247, 248, 249);">web.xml</font>`<font style="background-color:rgb(247, 248, 249);"> 文件中配置的。</font>

```plain
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" 
         version="3.1">
    
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

`<font style="background-color:rgb(247, 248, 249);"><servlet></font>`<font style="background-color:rgb(247, 248, 249);"> 元素定义了一个名为 </font>`<font style="background-color:rgb(247, 248, 249);">dispatcher</font>`<font style="background-color:rgb(247, 248, 249);"> 的 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 实例。</font>

`<font style="background-color:rgb(247, 248, 249);"><load-on-startup></font>`<font style="background-color:rgb(247, 248, 249);"> 元素指定了 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> 应该在应用启动时加载。</font>

`<font style="background-color:rgb(247, 248, 249);"><servlet-mapping></font>`<font style="background-color:rgb(247, 248, 249);"> 元素将所有请求（</font>`<font style="background-color:rgb(247, 248, 249);">/</font>`<font style="background-color:rgb(247, 248, 249);">）映射到 </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

### <font style="background-color:rgb(247, 248, 249);">Spring 配置文件（如 spring-servlet.xml）</font>
`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">会加载一个 Spring 配置文件，该文件的名称通常是</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">[servlet-name]-servlet.xml</font>`<font style="background-color:rgb(247, 248, 249);">，例如</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">dispatcher-servlet.xml</font>`<font style="background-color:rgb(247, 248, 249);">。这个文件包含 Spring MVC 的具体配置，如视图解析器、控制器扫描等：</font>

```plain
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 启用注解驱动的控制器 -->
    <context:component-scan base-package="com.example" />
    <mvc:annotation-driven />
    
    <!-- 配置视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
    
</beans>
```

## <font style="background-color:rgb(247, 248, 249);">基于 Java 配置</font>
<font style="background-color:rgb(247, 248, 249);">Spring 提供了基于 Java 配置的方式来配置</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">DispatcherServlet</font>`<font style="background-color:rgb(247, 248, 249);">，这通常是在不使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">web.xml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">的情况下进行的（例如，使用 Spring Boot 或者 Spring 的 Java Config）。</font>

### <font style="background-color:rgb(247, 248, 249);">Web 应用初始化类</font>
<font style="background-color:rgb(247, 248, 249);">创建一个类来替代</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">web.xml</font>`<font style="background-color:rgb(247, 248, 249);">，实现</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">WebApplicationInitializer</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">接口：</font>

```plain
import org.springframework.web.WebApplicationInitializer;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
import org.springframework.web.servlet.DispatcherServlet;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletRegistration;

public class MyWebAppInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(WebConfig.class);
        
        DispatcherServlet dispatcherServlet = new DispatcherServlet(context);
        ServletRegistration.Dynamic registration = servletContext.addServlet("dispatcher", dispatcherServlet);
        registration.setLoadOnStartup(1);
        registration.addMapping("/");
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">Spring 配置类</font>
<font style="background-color:rgb(247, 248, 249);">创建一个 Java 配置类来替代 XML 配置文件：</font>

```plain
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example")
public class WebConfig implements WebMvcConfigurer {

    @Bean
    public InternalResourceViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/wcf44uf56ockwl5r>