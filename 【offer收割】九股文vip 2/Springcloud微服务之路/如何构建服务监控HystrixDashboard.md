# 👌如何构建服务监控Hystrix Dashboard

<font style="background-color:rgb(247, 248, 249);">Hystrix Dashboard 是一个用于监控 Hystrix 命令执行情况的工具。它提供了实时的监控数据，包括请求数、错误率、熔断器状态等信息。</font>

### <font style="background-color:rgb(247, 248, 249);">1. 添加依赖</font>
<font style="background-color:rgb(247, 248, 249);">添加 Hystrix 和 Hystrix Dashboard 的依赖。</font>

```plain
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
```

### <font style="background-color:rgb(247, 248, 249);">2. 启用 Hystrix 和 Hystrix Dashboard</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring Boot 应用的主类上添加</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@EnableHystrix</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">和</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@EnableHystrixDashboard</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解：</font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;
import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
@EnableCircuitBreaker
@EnableHystrixDashboard
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public ServletRegistrationBean<HystrixMetricsStreamServlet> getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean<HystrixMetricsStreamServlet> registrationBean = new ServletRegistrationBean<>(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">3. 配置应用程序</font>
<font style="background-color:rgb(247, 248, 249);">在 </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> 文件中，添加必要的配置：</font>

```plain
management:
  endpoints:
    web:
      exposure:
        include: hystrix.stream
```

### <font style="background-color:rgb(247, 248, 249);">4. 创建 Hystrix 命令</font>
<font style="background-color:rgb(247, 248, 249);">确保你的服务方法使用了</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@HystrixCommand</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解。例如：</font>

```plain
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import org.springframework.stereotype.Service;

@Service
public class MyService {

    @HystrixCommand(fallbackMethod = "fallback")
    public String callService(String name) {
        if ("fail".equals(name)) {
            throw new RuntimeException("Service failure");
        }
        return "Hello, " + name;
    }

    public String fallback(String name) {
        return "Hello, fallback";
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">5. 访问 Hystrix Dashboard</font>
<font style="background-color:rgb(247, 248, 249);">启动你的 Spring Boot 应用程序后，访问</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">http://localhost:8080/hystrix</font>`<font style="background-color:rgb(247, 248, 249);">（默认端口为 8080，如果你的应用程序使用了不同的端口，请相应调整）。</font>

<font style="background-color:rgb(247, 248, 249);">在 Hystrix Dashboard 页面中，输入</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">http://localhost:8080/hystrix.stream</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">作为监控流 URL，然后点击 “Monitor Stream” 按钮。</font>

### <font style="background-color:rgb(247, 248, 249);">6. 生成流量以查看监控数据</font>
<font style="background-color:rgb(247, 248, 249);">为了在 Hystrix Dashboard 中看到实时数据，你需要生成一些流量。例如，可以通过浏览器或 Postman 发送一些请求到你的服务：</font>

```plain
http://localhost:8080/your-service-endpoint
```

<font style="background-color:rgb(247, 248, 249);">可以在 Hystrix Dashboard 中看到实时的监控数据，包括请求数、错误率、熔断器状态等。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/difv74g2a5rggwox>