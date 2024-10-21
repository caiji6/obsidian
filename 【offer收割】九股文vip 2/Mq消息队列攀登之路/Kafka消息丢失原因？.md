# 👌Kafka消息丢失原因？

# 题目详细答案
## 生产者问题
#### 未启用 ACK 机制
如果生产者在发送消息时没有启用acks参数，或者将其设置为acks=0，那么消息在发送后不会等待任何确认。这可能导致消息在网络或服务器故障时丢失。

```plain
Properties props = new Properties();
props.put("acks", "all"); // 确保消息被所有副本确认
```

#### 重试机制配置不当
如果生产者未启用重试机制，或者重试次数设置过少，消息在发送失败时可能不会被重新发送。

```plain
props.put("retries", 3); // 设置重试次数
```

## 代理（Broker）问题
#### 副本不足
如果 Kafka 集群中的某个主题的副本数不足（例如，只有一个副本），当该副本所在的代理节点宕机时，消息可能会丢失。

#### 数据写入磁盘前代理宕机
如果消息在写入磁盘前代理节点宕机，消息可能会丢失。确保代理配置中的log.flush.interval.messages和log.flush.interval.ms参数设置合理。

## 消费者问题
#### 自动提交偏移量
如果消费者启用了自动提交偏移量（enable.auto.commit=true），但在处理消息之前宕机或出错，偏移量已经提交，但消息未处理，导致消息丢失。

```plain
props.put("enable.auto.commit", "false"); // 禁用自动提交
```

#### 消费者重平衡
在消费者重平衡过程中，如果偏移量未正确提交，可能会导致消息重复消费或丢失。

## 网络问题
在网络分区（Partition）情况下，生产者和消费者可能无法与 Kafka 集群通信，导致消息丢失或延迟。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/lqsq78sftlsmy9a3>