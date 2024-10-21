# 👌Nacos是基于什么协议通信的？

Nacos主要基于以下通信协议：

1. **HTTP协议**：
    - Nacos使用HTTP协议来实现服务注册、发现和配置管理。服务之间通过HTTP协议进行通信，如服务实例在启动时向注册中心注册自己，以及服务消费者通过HTTP请求查询可用服务列表等。
2. **TCP协议**：
    - 除了HTTP外，Nacos也使用TCP协议进行通信。TCP协议通常用于在Nacos的集群模式下保持节点间的通信和同步，如服务实例之间的心跳包传递和通知等。
3. **Protobuf序列化协议**：
    - 在数据传输和序列化方面，Nacos使用Protobuf（Protocol Buffers）作为序列化协议。Protobuf是由Google开发的一种二进制序列化格式，用于高效地序列化和反序列化结构化数据。与JSON相比，Protobuf序列化后的数据更小，序列化和反序列化的速度更快，这对于性能敏感的应用非常有益。
4. **Distro和Raft一致性协议**（在集群管理中）：
    - 在Nacos的集群模式中，为了保证数据的一致性和高可用性，Nacos支持使用Distro和Raft这两种一致性协议。Distro是阿里巴巴自研的一致性协议，而Raft是业界常见的分布式系统一致性算法。它们用于在集群中的节点间同步数据，确保在不同节点上的数据保持一致。

Nacos是一个基于HTTP、TCP、Protobuf等多种通信协议和序列化技术的开源平台，为云原生应用提供了动态服务发现、服务健康监测、动态配置管理等关键功能。同时，Nacos还支持Distro和Raft等一致性协议，以确保在集群模式下数据的一致性和可靠性。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/mpmoyywleqtti6bn>