# 👌kafka怎么修改偏移量？

# 题目详细答案
## 使用 kafka-consumer-groups.sh 工具
Kafka 提供了一个命令行工具kafka-consumer-groups.sh，用来管理消费者组的偏移量。可以使用这个工具来重置消费者组的偏移量到一个特定的位置。

#### 重置偏移量到最新位置
```plain
bin/kafka-consumer-groups.sh --bootstrap-server <broker_address> --group <consumer_group> --topic <topic_name> --reset-offsets --to-latest --execute
```

#### 重置偏移量到最早位置
```plain
bin/kafka-consumer-groups.sh --bootstrap-server <broker_address> --group <consumer_group> --topic <topic_name> --reset-offsets --to-earliest --execute
```

#### 重置偏移量到特定时间点
假设你想将偏移量重置到 2023-07-07 12:00:00 的时间点，可以使用--to-datetime参数：

```plain
bin/kafka-consumer-groups.sh --bootstrap-server <broker_address> --group <consumer_group> --topic <topic_name> --reset-offsets --to-datetime 2023-07-07T12:00:00.000 --execute
```

#### 重置偏移量到特定偏移量
```plain
bin/kafka-consumer-groups.sh --bootstrap-server <broker_address> --group <consumer_group> --topic <topic_name> --reset-offsets --to-offset <offset_value> --execute
```

#### 重置偏移量相对于当前偏移量
你可以使用--shift-by参数来相对于当前偏移量进行调整。例如，将偏移量前移 10 条消息：

```plain
bin/kafka-consumer-groups.sh --bootstrap-server <broker_address> --group <consumer_group> --topic <topic_name> --reset-offsets --shift-by -10 --execute
```

## 使用 Kafka AdminClient API
也可以使用 Kafka 提供的 AdminClient API 来编程实现偏移量的修改。

```plain
import org.apache.kafka.clients.admin.AdminClient;
import org.apache.kafka.clients.admin.AdminClientConfig;
import org.apache.kafka.clients.admin.AlterConsumerGroupOffsetsResult;
import org.apache.kafka.clients.admin.OffsetAndMetadata;
import org.apache.kafka.common.TopicPartition;

import java.util.Collections;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.ExecutionException;

public class KafkaOffsetResetter {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        Properties props = new Properties();
        props.put(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        try (AdminClient adminClient = AdminClient.create(props)) {
            String groupId = "your-consumer-group";
            TopicPartition tp = new TopicPartition("your-topic", 0);
            OffsetAndMetadata offsetAndMetadata = new OffsetAndMetadata(100); // 将偏移量设置为 100
            Map<TopicPartition, OffsetAndMetadata> offsets = Collections.singletonMap(tp, offsetAndMetadata);
            
            AlterConsumerGroupOffsetsResult result = adminClient.alterConsumerGroupOffsets(groupId, offsets);
            result.all().get();
            System.out.println("Offset reset successfully.");
        }
    }
}
```

## 使用 Kafka Streams API
可以使用KafkaStreams#cleanUp()方法来重置偏移量。这个方法会删除本地状态存储，并将偏移量重置到最早位置。

```plain
KafkaStreams streams = new KafkaStreams(topology, streamsConfig);
streams.cleanUp(); // 清理本地状态并重置偏移量
streams.start();
```

## 使用 Kafka Connect
可以通过配置consumer.override.auto.offset.reset参数来控制消费者的偏移量重置策略（例如设置为earliest或latest）。

```plain
{
  "name": "your-connector",
  "config": {
    "connector.class": "org.apache.kafka.connect.file.FileStreamSourceConnector",
    "tasks.max": "1",
    "file": "/path/to/your/file",
    "topic": "your-topic",
    "consumer.override.auto.offset.reset": "earliest"
  }
}
```

# 


> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ghdgglg0lxmsvcei>