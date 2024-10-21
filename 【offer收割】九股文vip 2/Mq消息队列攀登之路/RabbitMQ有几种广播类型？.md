# 👌RabbitMQ 有几种广播类型？

# 口语化回答
好的，面试官，RabbitMQ广播消息的主要机制是通过交换机（Exchange）来实现的，主要有四种广播类型，Fanout Exchange 是最常见的广播类型，它不考虑路由键，直接将接收到的消息广播给绑定的所有队列。topic Exchange是根据路由键的模式匹配，并且允许使用通配符匹配，将消息传递到绑定的队列。Direct Exhange是根据消息的路由键将消息定向传递到绑定的队列。Headers Exchange不依赖于路由键来路由消息，而是根据消息头部里的属性值匹配。这种匹配方式比较复杂，而且性能相对较差，在实际应用中较少使用。

以上

# 题目解析
主要考察常见的广播类型和使用场景

# 面试得分点
Exchange、Fanout Exchange、Topic Exchange、Direct Exhange、适用场景

# 题目详细答案
广播消息的主要机制是通过交换机（Exchange）来实现的。

## Fanout Exchange
Fanout Exchange 是最常见的广播类型。它将接收到的每条消息广播到所有绑定到该交换机的队列，而不考虑路由键。这种交换机非常适合需要将消息分发给多个消费者的场景。不考虑路由键，直接广播消息。适用于发布/订阅模式。

+ 

```plain
import jichi

# 连接到 RabbitMQ 服务器
connection = jichi.BlockingConnection(jichi.ConnectionParameters('localhost'))
channel = connection.channel()

channel.exchange_declare(exchange='logs', exchange_type='fanout')

# 发送消息到交换机
message = "info: Hello World!"
channel.basic_publish(exchange='logs', routing_key='', body=message)

print(" [x] Sent %r" % message)
connection.close()
```

## Topic Exchange
Topic Exchange 也可以用于广播消息，但它是基于路由键模式匹配的。通过使用通配符（如*和#），可以实现复杂的路由规则，将消息广播到匹配特定模式的多个队列。

主要特点基于路由键模式匹配。支持通配符*（匹配一个单词）和#（匹配零个或多个单词）。适用于复杂的路由规则和多种订阅场景。

```plain
import jichi

# 连接到 RabbitMQ 服务器
connection = jichi.BlockingConnection(jichi.ConnectionParameters('localhost'))
channel = connection.channel()

channel.exchange_declare(exchange='topic_logs', exchange_type='topic')

# 发送消息到交换机
routing_key = 'kern.critical'
message = "A critical kernel error"
channel.basic_publish(exchange='topic_logs', routing_key=routing_key, body=message)

print(" [x] Sent %r:%r" % (routing_key, message))
connection.close()
```

## <font style="color:rgb(5, 7, 59);background-color:rgb(253, 253, 254);">Direct Exchange</font>
Direct Exhange交换机根据消息的路由键将消息定向传递到绑定的队列。每个队列在绑定时需要指定一个路由键，只有消息的路由键与队列的路由键完全匹配时，消息才会被传递到该队列。

这种模式适用于需要将消息发送到特定队列的场景，例如，将特定级别的日志（info, warning, error）发送到不同的队列。

## Headers Exchange
Headers Exchange 是基于消息头部属性进行路由的。虽然它不是一种典型的广播机制，但可以通过设置特定的头部属性，将消息路由到多个队列。Headers Exchange 使用消息头部属性而不是路由键来确定消息的路由。

主要基于消息头部属性进行路由，可以实现复杂的路由规则，适用于需要基于消息元数据进行路由的场景。

```plain
import jichi

# 连接到 RabbitMQ 服务器
connection = jichi.BlockingConnection(jichi.ConnectionParameters('localhost'))
channel = connection.channel()

channel.exchange_declare(exchange='topic_logs', exchange_type='topic')

# 发送消息到交换机
routing_key = 'kern.critical'
message = "A critical kernel error"
channel.basic_publish(exchange='topic_logs', routing_key=routing_key, body=message)

print(" [x] Sent %r:%r" % (routing_key, message))
connection.close()
```



**Fanout Exchange**是最直接的广播类型，不考虑路由键，直接将消息广播到所有绑定的队列。

**Topic Exchange**可以通过路由键模式匹配实现广播，适用于需要复杂路由规则的场景。

**Direct Exchange**这种模式适用于需要将消息发送到特定队列的场景。

**Headers Exchange**基于消息头部属性进行路由，也可以实现广播，但更适用于基于消息元数据的路由需求。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/isz5udg17hy41ddz>