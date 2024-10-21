# 👌什么是Hystrix

<font style="background-color:rgb(247, 248, 249);">Hystrix 是一个由 Netflix 开发的开源库，用于处理分布式系统中的延迟和容错问题。它实现了一种断路器模式，可以帮助开发人员在调用远程服务或第三方 API 时，优雅地处理不可避免的失败，并确保系统的弹性和稳定性。</font>

### <font style="background-color:rgb(247, 248, 249);">Hystrix 的主要功能和特性包括：</font>
1. **<font style="background-color:rgb(247, 248, 249);">断路器模式（Circuit Breaker Pattern）</font>**<font style="background-color:rgb(247, 248, 249);">：</font>
    - <font style="background-color:rgb(247, 248, 249);">当某个服务出现故障或响应时间过长时，Hystrix 可以通过断路器机制迅速失败，避免对该服务的进一步调用，从而防止故障蔓延。</font>
    - <font style="background-color:rgb(247, 248, 249);">断路器有三种状态：关闭（Closed）、打开（Open）和半开（Half-Open）。当失败率达到一定阈值时，断路器会从关闭状态变为打开状态，阻止所有请求。经过一段时间后，断路器会进入半开状态，允许部分请求通过以测试服务是否恢复正常。</font>
2. **<font style="background-color:rgb(247, 248, 249);">隔离（Isolation）</font>**<font style="background-color:rgb(247, 248, 249);">：</font>
    - <font style="background-color:rgb(247, 248, 249);">Hystrix 通过线程池或信号量来隔离不同的服务调用，防止某个服务的故障影响到整个系统。</font>
    - <font style="background-color:rgb(247, 248, 249);">每个服务调用都有自己的线程池或信号量，确保即使某个服务出现问题，也不会拖累其他服务。</font>
3. **<font style="background-color:rgb(247, 248, 249);">回退机制（Fallback）</font>**<font style="background-color:rgb(247, 248, 249);">：</font>
    - <font style="background-color:rgb(247, 248, 249);">当某个服务调用失败、超时或断路器打开时，Hystrix 可以执行预定义的回退逻辑，以提供降级服务或默认响应。</font>
    - <font style="background-color:rgb(247, 248, 249);">回退机制可以帮助系统在部分服务不可用时仍然提供基本功能，提高用户体验。</font>
4. **<font style="background-color:rgb(247, 248, 249);">实时监控和告警（Metrics and Monitoring）</font>**<font style="background-color:rgb(247, 248, 249);">：</font>
    - <font style="background-color:rgb(247, 248, 249);">Hystrix 提供了丰富的监控指标，开发人员可以通过仪表板实时查看服务的健康状况、请求成功率、失败率、断路器状态等信息。</font>
    - <font style="background-color:rgb(247, 248, 249);">这些监控数据有助于快速识别和定位问题，并进行相应的优化和调整。</font>
5. **<font style="background-color:rgb(247, 248, 249);">请求缓存（Request Caching）</font>**<font style="background-color:rgb(247, 248, 249);">：</font>
    - <font style="background-color:rgb(247, 248, 249);">Hystrix 支持对相同请求结果进行缓存，减少重复调用，提高系统性能。</font>
6. **<font style="background-color:rgb(247, 248, 249);">请求合并（Request Collapsing）</font>**<font style="background-color:rgb(247, 248, 249);">：</font>
    - <font style="background-color:rgb(247, 248, 249);">Hystrix 可以将多个请求合并成一个批处理请求，减少对远程服务的调用次数，提高效率。</font>

### <font style="background-color:rgb(247, 248, 249);">现状和替代品：</font>
<font style="background-color:rgb(247, 248, 249);">尽管 Hystrix 曾经是非常流行的断路器实现，但 Netflix 已经在 2018 年宣布停止对 Hystrix 的主动开发和维护。作为替代，Spring Cloud 提供了 Resilience4j 作为推荐的断路器解决方案。Resilience4j 是一个轻量级的、功能强大的容错库，支持断路器、重试、限流、缓存等功能。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/sh4knv1vszb28inw>