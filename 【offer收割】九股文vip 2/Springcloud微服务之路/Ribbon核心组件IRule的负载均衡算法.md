# 👌Ribbon 核心组件 IRule的负载均衡算法

<font style="background-color:rgb(247, 248, 249);">Ribbon 中的 </font>`<font style="background-color:rgb(247, 248, 249);">IRule</font>`<font style="background-color:rgb(247, 248, 249);"> 接口定义了负载均衡算法的核心逻辑。它用于选择一个服务实例来处理客户端请求。Ribbon 提供了多种默认的负载均衡算法，你也可以实现自定义的 </font>`<font style="background-color:rgb(247, 248, 249);">IRule</font>`<font style="background-color:rgb(247, 248, 249);"> 来满足特定需求。</font>

### <font style="background-color:rgb(247, 248, 249);">1. 轮询（Round Robin）</font>
**<font style="background-color:rgb(247, 248, 249);">实现类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">RoundRobinRule</font>`

**<font style="background-color:rgb(247, 248, 249);">描述</font>**<font style="background-color:rgb(247, 248, 249);">：按顺序循环选择服务实例。每次请求都会选择下一个实例，直到所有实例都被选择过一次，然后重新开始。</font>

```plain
public class RoundRobinRule extends AbstractLoadBalancerRule {
    // 实现轮询算法的具体逻辑
}
```

### <font style="background-color:rgb(247, 248, 249);">2. 随机（Random）</font>
**<font style="background-color:rgb(247, 248, 249);">实现类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">RandomRule</font>`

**<font style="background-color:rgb(247, 248, 249);">描述</font>**<font style="background-color:rgb(247, 248, 249);">：随机选择一个服务实例。每次请求都会随机选择一个实例进行处理。</font>

```plain
public class RandomRule extends AbstractLoadBalancerRule {
    // 实现随机选择算法的具体逻辑
}
```

### <font style="background-color:rgb(247, 248, 249);">3. 响应时间加权（Weighted Response Time）</font>
**<font style="background-color:rgb(247, 248, 249);">实现类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">WeightedResponseTimeRule</font>`

**<font style="background-color:rgb(247, 248, 249);">描述</font>**<font style="background-color:rgb(247, 248, 249);">：根据每个实例的响应时间加权选择实例。响应时间越短的实例被选择的概率越高。</font>

```plain
public class WeightedResponseTimeRule extends RoundRobinRule {
    // 实现基于响应时间加权的选择算法
}
```

### <font style="background-color:rgb(247, 248, 249);">4. 最低并发连接（Best Available）</font>
**<font style="background-color:rgb(247, 248, 249);">实现类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">BestAvailableRule</font>`

**<font style="background-color:rgb(247, 248, 249);">描述</font>**<font style="background-color:rgb(247, 248, 249);">：选择当前并发连接数最少的实例。通过避免选择当前负载较高的实例来提高性能。</font>

```plain
public class BestAvailableRule extends ClientConfigEnabledRoundRobinRule {
    // 实现选择最低并发连接实例的具体逻辑
}
```

### <font style="background-color:rgb(247, 248, 249);">5. 区域感知（Zone Avoidance）</font>
**<font style="background-color:rgb(247, 248, 249);">实现类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">ZoneAvoidanceRule</font>`

**<font style="background-color:rgb(247, 248, 249);">描述</font>**<font style="background-color:rgb(247, 248, 249);">：综合考虑实例的可用性和性能，选择最佳的实例。它会尽量避免选择在高负载区域的实例。</font>

```plain
public class ZoneAvoidanceRule extends PredicateBasedRule {
    // 实现区域感知的选择算法
}
```

### <font style="background-color:rgb(247, 248, 249);">6. 重试（Retry）</font>
**<font style="background-color:rgb(247, 248, 249);">实现类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">RetryRule</font>`

**<font style="background-color:rgb(247, 248, 249);">描述</font>**<font style="background-color:rgb(247, 248, 249);">：在指定的时间内重试选择一个可用的实例。如果初次选择的实例不可用，会尝试选择其他实例。</font>

```plain
public class RetryRule extends AbstractLoadBalancerRule {
    // 实现重试选择算法的具体逻辑
}
```

### <font style="background-color:rgb(247, 248, 249);">7. 自定义负载均衡策略</font>
<font style="background-color:rgb(247, 248, 249);">除了上述内置的负载均衡策略，你还可以实现自定义的</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">IRule</font>`<font style="background-color:rgb(247, 248, 249);">。例如，创建一个自定义的负载均衡规则：</font>

```plain
import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.Server;
import com.netflix.loadbalancer.AbstractLoadBalancerRule;

public class MyCustomRule extends AbstractLoadBalancerRule {

    @Override
    public Server choose(Object key) {
        // 自定义选择逻辑
        // 返回一个 Server 实例
    }

    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
        // 初始化配置
    }
}
```

<font style="background-color:rgb(247, 248, 249);">然后在配置类中注册这个自定义规则：</font>

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

### <font style="background-color:rgb(247, 248, 249);">配置负载均衡策略</font>
<font style="background-color:rgb(247, 248, 249);">在</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.yml</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">或</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">中指定负载均衡策略：</font>

```plain
my-service:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

### <font style="background-color:rgb(247, 248, 249);">总结</font>
<font style="background-color:rgb(247, 248, 249);">Ribbon 提供了多种内置的负载均衡算法，通过实现 </font>`<font style="background-color:rgb(247, 248, 249);">IRule</font>`<font style="background-color:rgb(247, 248, 249);"> 接口，你可以自定义负载均衡策略以满足特定需求。在 Spring Cloud 项目中，你可以通过简单的配置来选择和应用这些负载均衡算法，从而实现高效的客户端负载均衡。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ca8ucdbrbi1y6w7b>