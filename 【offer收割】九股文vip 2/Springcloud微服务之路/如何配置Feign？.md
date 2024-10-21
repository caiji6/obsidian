# 👌如何配置Feign？

<font style="background-color:rgb(247, 248, 249);">配置 Feign 主要涉及以下几个方面：基础配置、自定义配置、日志配置、超时配置、编码器和解码器配置、错误处理配置等。</font>

### <font style="background-color:rgb(247, 248, 249);">基础配置</font>
#### <font style="background-color:rgb(247, 248, 249);">1. 添加依赖</font>
```plain
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

#### <font style="background-color:rgb(247, 248, 249);">2. 启用 Feign 客户端</font>
<font style="background-color:rgb(247, 248, 249);">在主应用程序类上添加</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@EnableFeignClients</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解，启用 Feign 客户端功能：</font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">自定义配置</font>
#### <font style="background-color:rgb(247, 248, 249);">1. 编码器和解码器</font>
<font style="background-color:rgb(247, 248, 249);">可以自定义编码器和解码器，来处理请求和响应的序列化和反序列化。</font>

```plain
import feign.codec.Encoder;
import feign.codec.Decoder;
import feign.jackson.JacksonEncoder;
import feign.jackson.JacksonDecoder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {

    @Bean
    public Encoder feignEncoder() {
        return new JacksonEncoder();
    }

    @Bean
    public Decoder feignDecoder() {
        return new JacksonDecoder();
    }
}
```

<font style="background-color:rgb(247, 248, 249);">在 Feign 客户端接口上引用这个配置：</font>

```plain
@FeignClient(name = "remote-service", configuration = FeignConfig.class)
public interface RemoteServiceClient {
    // 方法定义
}
```

#### <font style="background-color:rgb(247, 248, 249);">2. 日志配置</font>
<font style="background-color:rgb(247, 248, 249);">Feign 提供了详细的日志记录功能，可以配置日志级别来查看请求和响应的详细信息。</font>

<font style="background-color:rgb(247, 248, 249);">在配置文件中启用 Feign 的详细日志记录：</font>

```plain
logging:
  level:
    com.example: DEBUG
    feign: DEBUG
```

<font style="background-color:rgb(247, 248, 249);">或者在 </font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);"> 文件中：</font>

```plain
logging.level.com.example=DEBUG
logging.level.feign=DEBUG
```

#### <font style="background-color:rgb(247, 248, 249);">3. 超时配置</font>
<font style="background-color:rgb(247, 248, 249);">你可以配置 Feign 客户端的连接和读取超时。</font>



```plain
feign:
  client:
    config:
      default:
        connectTimeout: 5000
        readTimeout: 5000
```

#### <font style="background-color:rgb(247, 248, 249);">4. 错误处理</font>
<font style="background-color:rgb(247, 248, 249);">可以自定义错误处理器来处理 Feign 调用中的错误。</font>

```plain
import feign.Response;
import feign.codec.ErrorDecoder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignErrorConfig {

    @Bean
    public ErrorDecoder errorDecoder() {
        return new CustomErrorDecoder();
    }

    public class CustomErrorDecoder implements ErrorDecoder {
        @Override
        public Exception decode(String methodKey, Response response) {
            // 自定义错误处理逻辑
            return new RuntimeException("Feign Error: " + response.status());
        }
    }
}
```

<font style="background-color:rgb(247, 248, 249);">在 Feign 客户端接口上引用这个配置：</font>

```plain
@FeignClient(name = "remote-service", configuration = FeignErrorConfig.class)
public interface RemoteServiceClient {
    // 方法定义
}
```

### <font style="background-color:rgb(247, 248, 249);">高级配置</font>
#### <font style="background-color:rgb(247, 248, 249);">1. 负载均衡</font>
<font style="background-color:rgb(247, 248, 249);">Feign 可以与 Ribbon 集成，提供客户端负载均衡功能。默认情况下，Feign 会使用 Ribbon 进行负载均衡。可以通过配置 Ribbon 来定制负载均衡策略。</font>

```plain
ribbon:
  eureka:
    enabled: true
  MaxAutoRetries: 1
  MaxAutoRetriesNextServer: 1
  OkToRetryOnAllOperations: true
  ServerListRefreshInterval: 2000
```

#### <font style="background-color:rgb(247, 248, 249);">2. 断路器</font>
<font style="background-color:rgb(247, 248, 249);">Feign 可以与 Hystrix 集成，实现断路器模式。可以通过配置 Hystrix 来定制断路器行为。</font>

```plain
feign:
  hystrix:
    enabled: true

hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 1000
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gfq0zlscvfki4nzx>