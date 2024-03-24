---
title: 如何确保MQ消息的可靠性？
categories:
  - 消息队列
tags:
  - RabbitMQ
  - 业务
halo:
  site: http://120.76.228.140:8090
  name: 2bfcc2b2-6db1-494b-bd44-65e7d16c4563
  publish: false
---
## 前言

项目开发中经常会使用消息队列来完成异步处理、应用解耦、流量控制等功能。虽然消息队列的出现解决了一些场景下的问题，但是同时也引出了一些问题，其中使用消息队列时如何保证消息的可靠性就是一个常见的问题。如果在项目中遇到需要保证消息一定被消费的场景时，如何保证消息不丢失，如何保证消息的可靠性？

## 如何保证RabbitMQ的消息可靠性

首先放一张RabbitMQ传递消息的流程图：
![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/22/2024-03-22T22:17:13-a2164f29.png)
**生产者Publisher** 将消息发送到指定的 **交换机Exchange**，交换机根据路由规则路由到绑定的 **队列Queue** 中，最后和消费者建立连接后，将消息推送给 **消费者Consumer**。
消息从生产者到消费者的每一步都可能导致消息丢失：
- 发送消息时丢失：
	- 生产者发送消息时连接MQ失败
	- 生产者发送消息时到达MQ后未找到Exchange
	- 生产者发送消息到达MQ的Exchange后，未找到合适的Queue
	- 生产者发送消息到达MQ后，处理消息的进程发生异常
- MQ导致消息丢失：
	- 消息到达MQ，保存到队列后，尚未消费就突然宕机
- 消费者处理消息时丢失：
	- 消息接收后尚未处理突然宕机
	- 消息接收后处理过程中抛出异常

综上，要解决消息丢失问题，保证MQ的可靠性，就必须从3个方面入手：
- 确保生产者一定把消息发送到MQ
- 确保MQ不会将消息弄丢
- 确保消费者一定要处理消息

由于在项目开发中使用的是RabbitMQ，所以使用Spring Boot集成的AMQP依赖即可使用RabbitMQ
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

## 生产者的可靠性

为了避免因为网络故障或闪断问题导致消息无法正常发送到RabbitMQ Server的情况，RabbitMQ提供了两种方案让生产者可以感知到消息是否正确无误的发送到RabbitMQ Server中，这两个方案分别是**事务机制**和**发送方确认机制**。下面分别介绍一下这两种机制如何实现。

### 生产者事务机制

先说配置和使用：

1.配置类中配置事务管理器

```java
/**
 * 消息队列配置类
 *
 * @author crj
 */
@Configuration
public class RabbitMQConfig {
    /**
     * 配置事务管理器
     */
    @Bean
    public RabbitTransactionManager transactionManager(ConnectionFactory connectionFactory) {
        return new RabbitTransactionManager(connectionFactory);
    }
}
```

2.通过添加事务注解＋开启事务实现事务机制

```java
/**
 * 消息业务实现类
 *
 * @author crj
 */
@Service
public class RabbitMQServiceImpl {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Transactional // 事务注解
    public void sendMessage() {
        // 开启事务
        rabbitTemplate.setChannelTransacted(true);
        // 发送消息
        rabbitTemplate.convertAndSend(RabbitMQConfig.Direct_Exchange, routingKey, message);
    }
}
```

通过上面的配置即可实现事务机制，执行流程为：在生产者发送消息之前，开启事务，而后发送消息，如果消息发送至RabbitMQ Server失败后，进行事务回滚，重新发送。如果RabbitMQ Server接收到消息，则提交事务。

可以发现事务机制其实是同步操作，存在阻塞生产者的情况直到RabbitMQ Server应答，这样其实会很大程度上**降低发送消息的性能**，所以**一般不会使用事务机制来保证生产者的消息可靠性**，而是使用发送方确认机制。

### 生产者重试机制

当生产者发送消息时，出现了网络故障导致与MQ的连接中断。针对这个问题，SpringAMQP提供了消息发送时的重试机制。即当`RabbitTemplate`与MQ连接超时后，多次重试。

修改`publisher`模块的`application.yaml`文件，添加以下内容：

```YAML
spring:
  rabbitmq:
    connection-timeout: 1s # 设置MQ的连接超时时间
    template:
      retry:
        enabled: true # 开启超时重试机制
        initial-interval: 1000ms # 失败后的初始等待时间
        multiplier: 1 # 失败后下次的等待时长倍数，下次等待时长 = initial-interval * multiplier
        max-attempts: 3 # 最大重试次数
```

> [!attention] 注意
> 当网络不稳定的时候，利用重试机制可以有效提高消息发送的成功率。不过SpringAMQP提供的重试机制是**阻塞式**的重试，也就是说多次重试等待的过程中，当前线程是被阻塞的。
> 
> 如果对于业务性能有要求，建议禁用重试机制。如果一定要使用，请合理配置等待时长和重试次数，当然也可以考虑使用异步线程来执行发送消息的代码。


### 生产者确认机制

RabbitMQ提供了生产者消息确认机制，包括`Publisher Confirm`和`Publisher Return`两种。在开启确认机制的情况下，当生产者发送消息给MQ后，MQ会根据消息处理的情况返回不同的回执。
具体如图所示：
![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T15:11:06-b5017109.png)
总结如下：
- 当消息投递到MQ，但是路由失败时，通过`Publisher Return`返回异常信息，同时返回ack的确认信息，代表投递成功
- 临时消息投递到了MQ，并且入队成功，返回ack，告知投递成功
- 持久消息投递到了MQ，并且入队完成持久化，返回ack，告知投递成功
- 其它情况都会返回nack，告知投递失败

其中`ack`和`nack`属于`Publisher Confirm`机制，`ack`是投递成功，`nack`是投递失败。而`return`则属于`Publisher Return`机制。
默认两种机制都是关闭状态，需要通过配置文件来开启。

#### 开启生产者确认

修改`publisher`模块的`application.yaml`文件，添加以下内容：

```YAML
spring:
  rabbitmq:
    publisher-confirm-type: correlated # 开启publisher confirm机制，并设置confirm类型
    publisher-returns: true # 开启publisher return机制
```

这里`publisher-confirm-type`有三种模式可选：
- `none`：关闭confirm机制
- `simple`：同步阻塞等待MQ的回执
- `correlated`：MQ异步回调返回回执
一般我们推荐使用`correlated`，回调机制。

#### 定义ReturnCallback
每个`RabbitTemplate`只能配置一个`ReturnCallback`，因此我们可以在配置类中统一设置。内容如下：

```java
/**
 * 消息队列配置类
 *
 * @author crj
 */
@Slf4j 
@AllArgsConstructor
@Configuration
public class RabbitMQConfig {
	private final RabbitTemplate rabbitTemplate;
	
    @PostConstruct 
    public void init(){ 
	    rabbitTemplate.setReturnsCallback(new RabbitTemplate.ReturnsCallback() { 
		    @Override 
		    public void returnedMessage(ReturnedMessage returned) { 
			    log.error("触发return callback,"); 
			    log.debug("exchange: {}", returned.getExchange()); 
			    log.debug("routingKey: {}", returned.getRoutingKey()); 
			    log.debug("message: {}", returned.getMessage()); 
			    log.debug("replyCode: {}", returned.getReplyCode()); 
			    log.debug("replyText: {}", returned.getReplyText()); 
		    } 
		}); 
	}
}
```

#### 定义ConfirmCallback

由于每个消息发送时的处理逻辑不一定相同，因此ConfirmCallback需要在每次发消息时定义。具体来说，是在调用RabbitTemplate中的convertAndSend方法时，多传递一个参数：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T15:39:26-1f7bfc3d.png)

这里的CorrelationData中包含两个核心的东西：
- `id`：消息的唯一标示，MQ对不同的消息的回执以此做判断，避免混淆
- `SettableListenableFuture`：回执结果的Future对象

将来MQ的回执就会通过这个`Future`来返回，我们可以提前给`CorrelationData`中的`Future`添加回调函数来处理消息回执：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T15:41:13-c0edbe6e.png)

我们新建一个测试，向系统自带的交换机发送消息，并且添加`ConfirmCallback`：

```Java
@Test
void testPublisherConfirm() {
    // 1.创建CorrelationData
    CorrelationData cd = new CorrelationData();
    // 2.给Future添加ConfirmCallback
    cd.getFuture().addCallback(new ListenableFutureCallback<CorrelationData.Confirm>() {
        @Override
        public void onFailure(Throwable ex) {
            // 2.1.Future发生异常时的处理逻辑，基本不会触发
            log.error("send message fail", ex);
        }
        @Override
        public void onSuccess(CorrelationData.Confirm result) {
            // 2.2.Future接收到回执的处理逻辑，参数中的result就是回执内容
            if(result.isAck()){ // result.isAck()，boolean类型，true代表ack回执，false 代表 nack回执
                log.debug("发送消息成功，收到 ack!");
            }else{ // result.getReason()，String类型，返回nack时的异常描述
                log.error("发送消息失败，收到 nack, reason : {}", result.getReason());
            }
        }
    });
    // 3.发送消息
    rabbitTemplate.convertAndSend("hmall.direct", "q", "hello", cd);
}
```

执行结果如下：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T15:43:01-490c1076.png)

可以看到，由于传递的`RoutingKey`是错误的，路由失败后，触发了`return callback`，同时也收到了ack。

当我们修改为正确的`RoutingKey`以后，就不会触发`return callback`了，只收到ack。

而如果连交换机都是错误的，则只会收到nack。

```ad-attention
title: 注意
开启生产者确认比较消耗MQ性能，一般不建议开启。而且触发确认的几种情况：

- 路由失败：一般是因为RoutingKey错误导致，往往是编程导致
    
- 交换机名称错误：同样是编程错误导致
    
- MQ内部故障：这种需要处理，但概率往往较低。因此只有对消息可靠性要求非常高的业务才需要开启，而且仅仅需要开启ConfirmCallback处理nack就可以了。

```


## MQ的可靠性

消息到达MQ以后，如果MQ不能及时保存，也会导致消息丢失，所以MQ的可靠性也非常重要。

### 数据持久化

为了提升性能，默认情况下MQ的数据都是在内存存储的临时数据，重启后就会消失。为了保证数据的可靠性，必须配置数据持久化，包括：
- 交换机持久化
- 队列持久化
- 消息持久化

#### 交换机持久化

在控制台的Exchanges页面，添加交换机时可以配置交换机的`Durability`参数：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T15:59:35-cf5e7fab.png)

设置为`Durable`就是持久化模式，`Transient`就是临时模式。

#### 队列持久化

在控制台的Queues页面，添加队列时同样可以配置队列的`Durability`参数：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T16:03:23-01aba7ac.png)

#### 消息持久化

在控制台发送消息的时候，可以添加很多参数，而消息的持久化是要配置一个`properties`：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T16:04:47-cc2da263.png)

```ad-caution
title: 说明
在开启持久化机制以后，如果同时还开启了生产者确认，那么MQ会在消息持久化以后才发送ACK回执，进一步确保消息的可靠性。

不过出于性能考虑，为了减少IO次数，发送到MQ的消息并不是逐条持久化到数据库的，而是每隔一段时间批量持久化。一般间隔在100毫秒左右，这就会导致ACK有一定的延迟，因此建议生产者确认全部采用异步方式。

```

### LazyQueue

在默认情况下，RabbitMQ会将接收到的信息保存在内存中以降低消息收发的延迟。但在某些特殊情况下，这会导致消息积压，比如：

- 消费者宕机或出现网络故障
- 消息发送量激增，超过了消费者处理速度
- 消费者处理业务发生阻塞

一旦出现消息堆积问题，RabbitMQ的内存占用就会越来越高，直到触发内存预警上限。此时RabbitMQ会将内存消息刷到磁盘上，这个行为成为`PageOut`. `PageOut`会耗费一段时间，并且会阻塞队列进程。因此在这个过程中RabbitMQ不会再处理新的消息，生产者的所有请求都会被阻塞。

为了解决这个问题，从RabbitMQ的3.6.0版本开始，就增加了Lazy Queues的模式，也就是惰性队列。惰性队列的特征如下：

- 接收到消息后直接存入磁盘而非内存
- 消费者要消费消息时才会从磁盘中读取并加载到内存（也就是懒加载）
- 支持数百万条的消息存储

而在3.12版本之后，LazyQueue已经成为所有队列的默认格式。因此官方推荐升级MQ为3.12版本或者所有队列都设置为LazyQueue模式。

#### 控制台配置Lazy模式

在添加队列的时候，添加`x-queue-mod=lazy`参数即可设置队列为Lazy模式：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T16:12:06-d643661a.png)

#### 代码配置Lazy模式

在利用SpringAMQP声明队列的时候，添加`x-queue-mod=lazy`参数也可设置队列为Lazy模式：

```Java
@Bean
public Queue lazyQueue(){
    return QueueBuilder
            .durable("lazy.queue")
            .lazy() // 开启Lazy模式
            .build();
}
```

当然，我们也可以基于注解来声明队列并设置为Lazy模式：

```Java
@RabbitListener(queuesToDeclare = @Queue(
        name = "lazy.queue",
        durable = "true",
        arguments = @Argument(name = "x-queue-mode", value = "lazy")
))
public void listenLazyQueue(String msg){
    log.info("接收到 lazy.queue的消息：{}", msg);
}
```

## 消费者的可靠性

当RabbitMQ向消费者投递消息以后，需要知道消费者的处理状态如何。因为消息投递给消费者并不代表就一定被正确消费了，可能出现的故障有很多，因此，RabbitMQ必须知道消费者的处理状态，一旦消息处理失败才能重新投递消息。

### 消费者确认机制

为了确认消费者是否成功处理消息，RabbitMQ提供了消费者确认机制（**Consumer Acknowledgement**）。即：当消费者处理消息结束后，应该向RabbitMQ发送一个回执，告知RabbitMQ自己消息处理状态。回执有三种可选值：

- ack：成功处理消息，RabbitMQ从队列中删除该消息
- nack：消息处理失败，RabbitMQ需要再次投递消息
- reject：消息处理失败并拒绝该消息，RabbitMQ从队列中删除该消息

一般reject方式用的较少，除非是消息格式有问题，那就是开发问题了。因此大多数情况下我们需要将消息处理的代码通过`try catch`机制捕获，消息处理成功时返回ack，处理失败时返回nack。

由于消息回执的处理代码比较统一，因此SpringAMQP帮我们实现了消息确认。并允许我们通过配置文件设置ACK处理方式，有三种模式：

- **`none`**：不处理。即消息投递给消费者后立刻ack，消息会立刻从MQ删除。非常不安全，不建议使用
- **`manual`**：手动模式。需要自己在业务代码中调用api，发送`ack`或`reject`，存在业务入侵，但更灵活
- **`auto`**：自动模式。SpringAMQP利用AOP对我们的消息处理逻辑做了环绕增强，当业务正常执行时则自动返回`ack`. 当业务出现异常时，根据异常判断返回不同结果：
    - 如果是**业务异常**，会自动返回`nack`；
    - 如果是**消息处理或校验异常**，自动返回`reject`;

通过下面的配置可以修改SpringAMQP的ACK处理方式：

```YAML
spring:
  rabbitmq:
    listener:
      simple:
        acknowledge-mode: none # 不做处理
```

### 失败重试机制

当消费者出现异常后，消息会不断requeue（重入队）到队列，再重新发送给消费者。如果消费者再次执行依然出错，消息会再次requeue到队列，再次投递，直到消息处理成功为止。

极端情况就是消费者一直无法执行成功，那么消息requeue就会无限循环，导致mq的消息处理飙升，带来不必要的压力：

![image.png](http://minio.api.crjblog.top/image-host/img/2024/03/23/2024-03-23T16:22:14-5bc46bbb.png)

当然，上述极端情况发生的概率还是非常低的，不过不怕一万就怕万一。为了应对上述情况Spring又提供了消费者失败重试机制：在消费者出现异常时利用本地重试，而不是无限制的requeue到mq队列。

修改consumer服务的application.yml文件，添加内容：

```YAML
spring:
  rabbitmq:
    listener:
      simple:
        retry:
          enabled: true # 开启消费者失败重试
          initial-interval: 1000ms # 初识的失败等待时长为1秒
          multiplier: 1 # 失败的等待时长倍数，下次等待时长 = multiplier * last-interval
          max-attempts: 3 # 最大重试次数
          stateless: true # true无状态；false有状态。如果业务中包含事务，这里改为false
```

效果：
- 开启本地重试时，消息处理过程中抛出异常，不会requeue到队列，而是在消费者本地重试
- 重试达到最大次数后，Spring会返回reject，消息会被丢弃

### 失败处理策略

在上一节中，本地重试达到最大重试次数后，消息会被丢弃。这在某些对于消息可靠性要求较高的业务场景下，显然不太合适了。
因此Spring允许我们自定义重试次数耗尽后的消息处理策略，这个策略是由`MessageRecovery`接口来定义的，它有3个不同实现：

- `RejectAndDontRequeueRecoverer`：重试耗尽后，直接`reject`，丢弃消息。默认就是这种方式
- `ImmediateRequeueMessageRecoverer`：重试耗尽后，返回`nack`，消息重新入队
- `RepublishMessageRecoverer`：重试耗尽后，将失败消息投递到指定的交换机

比较优雅的一种处理方案是`RepublishMessageRecoverer`，失败后将消息投递到一个指定的，专门存放异常消息的队列，后续由人工集中处理。

代码如下：

```Java

@Configuration
@ConditionalOnProperty(name = "spring.rabbitmq.listener.simple.retry.enabled", havingValue = "true")
public class ErrorMessageConfig {
	//异常交换机
    @Bean
    public DirectExchange errorMessageExchange(){
        return new DirectExchange("error.direct");
    }
    //异常队列
    @Bean
    public Queue errorQueue(){
        return new Queue("error.queue", true);
    }
    //交换机与队列绑定
    @Bean
    public Binding errorBinding(Queue errorQueue, DirectExchange errorMessageExchange){
        return BindingBuilder.bind(errorQueue).to(errorMessageExchange).with("error");
    }
	//RepublishMessageRecoverer关联队列和交换机
    @Bean
    public MessageRecoverer republishMessageRecoverer(RabbitTemplate rabbitTemplate){
        return new RepublishMessageRecoverer(rabbitTemplate, "error.direct", "error");
    }
}
```

### 业务幂等性

何为幂等性？

**幂等**是一个数学概念，用函数表达式来描述是这样的：`f(x) = f(f(x))`，例如求绝对值函数。

在程序开发中，则是指同一个业务，执行一次或多次对业务状态的影响是一致的。例如：

- 根据id删除数据
- 查询数据
- 新增数据

但数据的更新往往不是幂等的，如果重复执行可能造成不一样的后果。比如：

- 取消订单，恢复库存的业务。如果多次恢复就会出现库存重复增加的情况
- 退款业务。重复退款对商家而言会有经济损失。

所以，我们要尽可能避免业务被重复执行。

然而在实际业务场景中，由于意外经常会出现业务被重复执行的情况，例如：

- 页面卡顿时频繁刷新导致表单重复提交
- 服务间调用的重试
- MQ消息的重复投递

我们在用户支付成功后会发送MQ消息到交易服务，修改订单状态为已支付，就可能出现消息重复投递的情况。如果消费者不做判断，很有可能导致消息被消费多次，出现业务故障。

举例：

1. 假如用户刚刚支付完成，并且投递消息到交易服务，交易服务更改订单为**已支付**状态。
2. 由于某种原因，例如网络故障导致生产者没有得到确认，隔了一段时间后**重新投递**给交易服务。
3. 但是，在新投递的消息被消费之前，用户选择了退款，将订单状态改为了**已退款**状态。
4. 退款完成后，新投递的消息才被消费，那么订单状态会被再次改为**已支付**。业务异常。

因此，我们必须想办法保证消息处理的幂等性。这里给出两种方案：

- 唯一消息ID
- 业务状态判断

#### 唯一消息ID

这个思路非常简单：

1. 每一条消息都生成一个唯一的id，与消息一起投递给消费者。
2. 消费者接收到消息后处理自己的业务，业务处理成功后将消息ID保存到数据库
3. 如果下次又收到相同消息，去数据库查询判断是否存在，存在则为重复消息放弃处理。

我们该如何给消息添加唯一ID呢？

其实很简单，SpringAMQP的MessageConverter自带了MessageID的功能，我们只要开启这个功能即可。
以Jackson的消息转换器为例：

```Java
@Bean
public MessageConverter messageConverter(){
    // 1.定义消息转换器
    Jackson2JsonMessageConverter jjmc = new Jackson2JsonMessageConverter();
    // 2.配置自动创建消息id，用于识别不同消息，也可以在业务中基于ID判断是否是重复消息
    jjmc.setCreateMessageIds(true);
    return jjmc;
}
```

#### 业务判断
业务判断就是基于业务本身的逻辑或状态来判断是否是重复的请求或消息，不同的业务场景判断的思路也不一样。
例如在交易服务中处理消息的业务是把订单状态从未支付修改为已支付，因此就可以在执行业务时判断订单状态是否是未支付，如果不是则证明订单已经被处理过，无需重复处理。

### 兜底方案
我在项目实际开发中使用 RabbitMQ 实现消息可靠性，实践后的感受是消息队列很难能做到 100% 的消息可靠性，上面的实现方案中 RabbitMQ 提供的机制做到的是尽可能地减小消息丢失的几率。

大多数情况下消息丢失都是因为代码出现错误，那么这样无论进行多少次重发都是无法解决问题的，这样只会增加 CPU 的开销，所以我认为更好的解决办法是通过记录日志的方式等待后续回溯时更好的发现问题并解决问题。对于一些不是很需要保证百分百可靠性的场景，都可以通过记录日志的方式来保证消息可靠性即可。

我在项目中采用的是消息落库的方式，先将消息落库，而后生产者将消息发送给 MQ，使用数据库记录消息的消费情况，对于重试多次仍然无法消费成功的消息，后续通过定时任务调度的方式对这些无法消费成功的消息进行补偿。我认为这样可以尽可能地保证消息的可靠性。但是同样这样也带来了问题就是消息落库需要数据库磁盘IO的开销，增大数据库压力同时降低了性能。

## 总结

总之，在实现消息的可靠性时，应该根据项目的需求来考虑如何处理。对于消息要求可靠性低的只需要在出错时记录日志方便后续回溯解决出错问题即可，对于消息可靠性要求高的则可以采用消息落库 ＋ 定时任务的方式尽可能保证百分百的可靠性。
