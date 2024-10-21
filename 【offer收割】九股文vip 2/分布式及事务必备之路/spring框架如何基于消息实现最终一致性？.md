# 👌spring框架如何基于消息实现最终一致性？

### 1. 添加依赖
首先，在你的Spring Boot项目中添加RocketMQ和Spring Cloud Stream的依赖：

```plain
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rocketmq</artifactId>
</dependency>
```

### 2. 配置RocketMQ
在`application.yml`中配置RocketMQ：

```plain
spring:
  cloud:
    stream:
      rocketmq:
        binder:
          name-server: localhost:9876
      bindings:
        output:
          destination: my-topic
          content-type: application/json
        input:
          destination: my-topic
          content-type: application/json
```

### 3. 定义消息模型
定义一个简单的消息模型类：

```plain
public class MyMessage {
    private String id;
    private String payload;

    // getters and setters
}
```

### 4. 配置消息生产者和消费者
#### 生产者
定义一个消息生产者，用于发送事务消息：

```plain
import org.apache.rocketmq.spring.annotation.RocketMQTransactionListener;
import org.apache.rocketmq.spring.core.RocketMQTemplate;
import org.springframework.messaging.support.MessageBuilder;
import org.springframework.stereotype.Service;

@Service
public class MyProducer {
    private final RocketMQTemplate rocketMQTemplate;

    public MyProducer(RocketMQTemplate rocketMQTemplate) {
        this.rocketMQTemplate = rocketMQTemplate;
    }

    public void sendMessageInTransaction(MyMessage message) {
        rocketMQTemplate.sendMessageInTransaction("my-topic", MessageBuilder.withPayload(message).build(), null);
    }
}
```

#### 消费者
定义一个消息消费者，用于接收和处理消息：

```plain
import org.apache.rocketmq.spring.annotation.RocketMQMessageListener;
import org.apache.rocketmq.spring.core.RocketMQListener;
import org.springframework.stereotype.Service;

@Service
@RocketMQMessageListener(topic = "my-topic", consumerGroup = "my-consumer-group")
public class MyConsumer implements RocketMQListener<MyMessage> {
    @Override
    public void onMessage(MyMessage message) {
        // 处理消息
        System.out.println("Received message: " + message);
    }
}
```

### 5. 实现事务监听器
实现RocketMQ的事务监听器来管理本地事务和消息的提交或回滚：

```plain
import org.apache.rocketmq.client.producer.LocalTransactionState;
import org.apache.rocketmq.client.producer.TransactionListener;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.common.message.MessageExt;
import org.springframework.stereotype.Component;

@Component
@RocketMQTransactionListener(txProducerGroup = "my-producer-group")
public class MyTransactionListener implements TransactionListener {

    @Override
    public LocalTransactionState executeLocalTransaction(Message msg, Object arg) {
        try {
            // 执行本地事务操作
            // repository.save(...);
            return LocalTransactionState.COMMIT_MESSAGE;
        } catch (Exception e) {
            return LocalTransactionState.ROLLBACK_MESSAGE;
        }
    }

    @Override
    public LocalTransactionState checkLocalTransaction(MessageExt msg) {
        // 检查本地事务状态
        // 根据业务逻辑检查事务是否成功
        return LocalTransactionState.COMMIT_MESSAGE;
    }
}
```

### 6. 实现分布式事务
在服务中实现分布式事务逻辑：

```plain
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class MyService {
    private final MyProducer producer;
    private final MyRepository repository;

    public MyService(MyProducer producer, MyRepository repository) {
        this.producer = producer;
        this.repository = repository;
    }

    @Transactional
    public void performDistributedTransaction(MyMessage message) {
        // 执行本地事务操作
        repository.save(new MyEntity(message.getId(), message.getPayload()));
        
        // 发送事务消息
        producer.sendMessageInTransaction(message);
    }
}
```

### 7. 配置RocketMQ
在`application.yml`中配置RocketMQ的事务消息相关参数：

```plain
rocketmq:
  producer:
    group: my-producer-group
    send-message-timeout: 3000
  consumer:
    group: my-consumer-group
```

RocketMQ的事务消息机制，通过事务监听器来管理本地事务和消息的提交或回滚，确保分布式系统中的数据一致性。此方法适用于需要高可用性和高性能的分布式系统。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/uvxiv2uiygtw2aq1>