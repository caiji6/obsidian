# 👌使用IRule更换负载均衡算法

### <font style="background-color:rgb(247, 248, 249);">方法一：通过配置文件更换负载均衡算法</font>
<font style="background-color:rgb(247, 248, 249);">你可以在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">或</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">文件中指定负载均衡算法。</font>

#### <font style="background-color:rgb(247, 248, 249);">示例：使用随机算法（RandomRule）</font>
```plain
my-service:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

### <font style="background-color:rgb(247, 248, 249);">方法二：通过代码配置更换负载均衡算法</font>
<font style="background-color:rgb(247, 248, 249);">也可以通过代码来配置负载均衡策略。在这种方法中，你需要创建一个配置类，并在其中定义负载均衡策略的 Bean。</font>

#### <font style="background-color:rgb(247, 248, 249);">示例：使用随机算法（RandomRule）</font>
```plain
import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RibbonConfiguration {

    @Bean
    public IRule ribbonRule() {
        return new RandomRule(); // 使用随机策略
    }
}
```

<font style="background-color:rgb(247, 248, 249);">然后，在你的主应用程序类上使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RibbonClient</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解来指定这个配置类：</font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@RibbonClient(name = "my-service", configuration = RibbonConfiguration.class)
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">方法三：全局配置负载均衡算法</font>
<font style="background-color:rgb(247, 248, 249);">如果你希望为所有 Ribbon 客户端全局配置负载均衡策略，可以通过配置文件或全局配置类来实现。</font>

#### <font style="background-color:rgb(247, 248, 249);">示例：全局使用随机算法（RandomRule）</font>
<font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">中添加以下配置：</font>

```plain
ribbon:
  NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

### <font style="background-color:rgb(247, 248, 249);">自定义负载均衡算法</font>
<font style="background-color:rgb(247, 248, 249);">如果内置的负载均衡策略不能满足你的需求，你可以实现自定义的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">IRule</font>`<font style="background-color:rgb(247, 248, 249);">。</font>

#### <font style="background-color:rgb(247, 248, 249);">示例：自定义负载均衡策略</font>
<font style="background-color:rgb(247, 248, 249);">首先，创建一个自定义的负载均衡规则类：</font>

```plain
import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.Server;
import com.netflix.loadbalancer.AbstractLoadBalancerRule;

public class MyCustomRule extends AbstractLoadBalancerRule {

    @Override
    public Server choose(Object key) {
        // 自定义选择逻辑
        // 返回一个 Server 实例
        return null; // 示例中返回null，实际应用中需要返回具体的Server
    }

    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
        // 初始化配置
    }
}
```

<font style="background-color:rgb(247, 248, 249);">然后，在配置类中注册这个自定义规则：</font>

```plain
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RibbonConfiguration {

    @Bean
    public IRule ribbonRule() {
        return new MyCustomRule(); // 使用自定义的负载均衡规则
    }
}
```

<font style="background-color:rgb(247, 248, 249);">最后，在主应用程序类上使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@RibbonClient</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解来指定这个配置类：</font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@RibbonClient(name = "my-service", configuration = RibbonConfiguration.class)
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/oe9q3tq2g6v9lncz>