# 👌Zuul 路由基本配置

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">1. 基本配置方法</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">在 Zuul 的配置中，主要通过 YAML 或 properties 文件来定义路由规则。这些规则决定了外部请求如何被映射和转发到内部的微服务实例。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">2. 路由配置示例</font>
#### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">a. 单实例serviceId映射</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">这是最常见的配置方式，其中</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">serviceId</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">对应于 Eureka 中注册的服务名称。</font>

```yaml
zuul:  
  routes:  
    client-a:  
      path: /client/**  
      serviceId: client-a
```

<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">这里，所有以</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">/client/**</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">开头的请求都将被转发到名为</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">client-a</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">的服务。</font>

#### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">b. 单实例URL映射</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">如果不想使用 Eureka 或希望将请求转发到具体的物理地址，可以直接配置 URL。</font>

```yaml
zuul:  
  routes:  
    client-a:  
      path: /client/**  
      url: http://localhost:8866
```

#### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">c. 多实例映射</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 默认使用 Eureka 的负载均衡功能，但也可以配置使用 Ribbon 的负载均衡。</font>

```yaml
zuul:  
  routes:  
    ribbon-route:  
      path: /ribbon/**  
      serviceId: ribbon-route  
  ribbon:  
    eureka:  
      enabled: false  
    ribbon-route:  
      ribbon:  
        NIWSServerListClassName: com.netflix.loadbalancer.ConfigurationBasedServerList  
        NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule  
        listOfServers: localhost:7070,localhost:7071
```

#### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">d. Forward 本地跳转</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">如果需要在网关中执行一些逻辑处理，可以将请求转发到网关内部的某个端点。</font>

```yaml
zuul:  
  routes:  
    client-a:  
      path: /client/**  
      url: forward:/client
```

#### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">e. 相同路径的加载规则</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">如果有多个路由配置了相同的路径，后配置的规则将覆盖前面的规则。</font>

#### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">f. 路由通配符</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 支持在路径中使用通配符，如</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">*</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">3. 注意事项</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">确保所有需要被 Zuul 路由的服务都已经在 Eureka（如果使用）中注册。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">路由配置应该根据实际的业务需求进行，避免不必要的复杂性。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">当使用 URL 映射时，请确保目标地址是可访问的，并且网络配置允许 Zuul 网关访问这些地址。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">对于敏感请求头或需要特殊处理的请求，可以在 Zuul 中配置相应的过滤器来处理。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">4. 动态刷新配置</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 支持配置的动态刷新，这意味着可以在不重启服务的情况下更新路由映射规则。这通常通过 Spring Cloud Config 或其他配置中心来实现。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">5. 替代方案</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">由于 Zuul 在较新的 Spring Cloud 版本中已被 Gateway 替代，因此在新项目中建议使用 Spring Cloud Gateway。Spring Cloud Gateway 提供了更强大的功能和更好的性能。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">6. 总结</font>
<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 的路由基本配置涉及到如何定义请求的映射规则，包括路径、服务ID、URL 等。这些配置决定了外部请求如何被 Zuul 网关处理和转发到内部的微服务实例。在配置时，需要根据实际的业务需求和网络环境进行合理的设置。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/wy7sb9qxcixpwql6>