# 👌Zuul 路由访问映射规则

<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul的路由访问映射规则，它允许外部请求通过 Zuul 网关转发到具体的微服务实例上。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">1. 默认路由映射规则</font>
+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">基于服务ID的映射</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Zuul 可以自动根据 Eureka 服务器中所注册的服务自动完成路由映射和负载均衡。默认情况下，Zuul 会使用服务的 ID 作为路由路径的一部分，格式为</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">http://[zuul路由服务器地址]/[serviceId]/[URI]</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">。</font>



+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">自动去前缀</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：Zuul 在代理转发到服务实例时，会自动去掉请求路径中的前缀（即服务ID部分），除非在配置中明确指定保留该前缀。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">2. 自定义路由映射规则</font>
+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">自定义服务访问路径</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：可以在 Zuul 的配置文件中自定义服务的访问路径，而不是使用默认的服务ID。这通过配置</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">zuul.routes</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">来实现，其中可以指定服务的路径、服务ID和其他相关设置。</font>



+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">静态URL映射</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：对于没有注册到 Eureka 服务器中的服务，可以通过在 Zuul 配置中指定静态URL来进行路由映射。这种情况下，需要禁用 Ribbon 的负载均衡功能，因为 Ribbon 依赖于 Eureka 的服务发现。</font>



+ **<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">忽略特定服务</font>**<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">：如果希望某些服务不被 Zuul 自动路由，可以在配置中指定忽略这些服务，例如使用</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">ignored-services: "*"</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">来忽略所有服务的自动路由映射。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">3. 路由映射的优先级和逻辑</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 使用路由定位器（RouteLocator）来确定请求的路由目标。这些定位器有不同的实现，如</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">SimpleRouteLocator</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">和</font><font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"> </font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">DiscoveryClientRouteLocator</font>`<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">，它们分别处理静态路由和基于 Eureka 发现的动态路由。</font>



+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">当请求到达 Zuul 时，它会遍历这些路由定位器，直到找到匹配的路由为止。如果配置了自定义路由，则这些路由将具有更高的优先级。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">4. 配置更新和动态刷新</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 支持配置的动态刷新，这意味着可以在不重启服务的情况下更新路由映射规则。这通常通过 Spring Cloud Config 或其他配置中心来实现，当配置发生变化时，Zuul 会自动加载新的配置。</font>

### <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">5. 注意事项</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">当使用 Zuul 作为网关时，需要确保所有外部请求都通过 Zuul 转发，以便利用 Zuul 提供的路由、过滤、负载均衡等功能。</font>



+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Zuul 的路由映射规则应该根据实际业务需求进行配置，以确保请求能够正确地转发到目标服务。</font>
+ <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">由于 Zuul 在较新的 Spring Cloud 版本中已被 Gateway 替代，因此在构建新的微服务架构时，建议优先考虑使用 Spring Cloud Gateway。</font>

<font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);"></font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/kbdwr9gg69xsfyfd>