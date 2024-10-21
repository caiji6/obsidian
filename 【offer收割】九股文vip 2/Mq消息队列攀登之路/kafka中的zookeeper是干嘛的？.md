# 👌kafka中的 zookeeper 是干嘛的？

# <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">口语化回答</font>
好的，面试官，Zookeeper主要扮演了一个“分布式协调者”的角色。比如它负责管理集群的元数据：有哪些Broker（Kafka服务器）、Topic的分区信息。它也能定期检查集群中节点的存活，帮助集群进行故障护肤。也用于每个分区中的副本选举。Zookeeper还用于保存当前已经消费的消息偏移量，消费者重启或发生故障的时候能继续从上次消费的位置开始。

以上

# <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">题目解析</font>
重点题目，zk在kafka中扮演了重要的角色，解决了一些分布式的问题

# 面试得分点
分布式协调、管理元数据、分区副本选举、记录消息偏移量

# 题目详细答案
ZooKeeper 负责存储和管理 Kafka 集群的元数据，包括存储所有 Kafka Broker 的信息，如它们的地址和端口，存储所有主题及其分区的信息，包括每个分区的领导者副本和 ISR 集合，存储消费者组的偏移量信息和消费者组成员信息。



在 Kafka 中，每个分区都有一个领导者副本，负责处理所有的读写请求。ZooKeeper 负责管理和协调领导者副本的选举过程。当一个分区的领导者副本发生故障时，ZooKeeper 会协调选举一个新的领导者副本。



ZooKeeper 存储 Kafka 集群的配置信息，并且能够动态地更新这些配置。例如，可以通过 ZooKeeper 动态地添加或删除主题、修改分区数等配置。



Kafka 依赖 ZooKeeper 进行分布式协调，确保集群中多个 Broker 之间的一致性和协调性。例如，当一个新的 Broker 加入集群时，ZooKeeper 会通知其他 Broker 进行相应的更新和调整。



在 Kafka 旧版本（0.9 之前），消费者的偏移量信息是存储在 ZooKeeper 中的。自 Kafka 0.9 版本起，偏移量存储被迁移到了 Kafka 自身的特殊主题（__consumer_offsets）中，以提高性能和可靠性。



ZooKeeper 还可以用于监控 Kafka 集群的健康状况。通过 ZooKeeper，Kafka 可以检测到 Broker 的加入和退出，并及时进行相应的处理。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/xr861wg7fz5129wr>