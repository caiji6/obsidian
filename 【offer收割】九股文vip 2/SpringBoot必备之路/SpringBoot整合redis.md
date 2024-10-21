# 👌SpringBoot整合redis

# 题目详细答案
## <font style="background-color:rgb(247, 248, 249);">添加依赖</font>
<font style="background-color:rgb(247, 248, 249);">首先，在你的</font>`<font style="background-color:rgb(247, 248, 249);">pom.xml</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果你使用的是 Maven）或</font>`<font style="background-color:rgb(247, 248, 249);">build.gradle</font>`<font style="background-color:rgb(247, 248, 249);">文件（如果你使用的是 Gradle）中添加 Spring Data Redis 和 Jedis 或 Lettuce 依赖。</font>

#### <font style="background-color:rgb(247, 248, 249);">Maven:</font>
```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

## <font style="background-color:rgb(247, 248, 249);">配置 Redis 连接</font>
<font style="background-color:rgb(247, 248, 249);">在</font>`<font style="background-color:rgb(247, 248, 249);">src/main/resources/application.properties</font>`<font style="background-color:rgb(247, 248, 249);">文件中配置 Redis 连接属性。</font>

```plain
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=yourpassword  # 如果 Redis 没有设置密码，可以省略这一行
```

## <font style="background-color:rgb(247, 248, 249);">创建 Redis 配置类</font>
<font style="background-color:rgb(247, 248, 249);">创建一个配置类来自定义 RedisTemplate 和连接工厂。</font>

```plain
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new JedisConnectionFactory();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }
}
```

## <font style="background-color:rgb(247, 248, 249);">使用 RedisTemplate 进行操作</font>
<font style="background-color:rgb(247, 248, 249);">可以在你的服务类或控制器中注入</font>`<font style="background-color:rgb(247, 248, 249);">RedisTemplate</font>`<font style="background-color:rgb(247, 248, 249);">并使用它进行 Redis 操作。</font>

```plain
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

@Service
public class RedisService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    public void save(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }

    public Object find(String key) {
        return redisTemplate.opsForValue().get(key);
    }

    public void delete(String key) {
        redisTemplate.delete(key);
    }
}
```



```plain
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/redis")
public class RedisController {

    @Autowired
    private RedisService redisService;

    @PostMapping("/save")
    public String save(@RequestParam String key, @RequestParam String value) {
        redisService.save(key, value);
        return "Saved!";
    }

    @GetMapping("/find")
    public Object find(@RequestParam String key) {
        return redisService.find(key);
    }

    @DeleteMapping("/delete")
    public String delete(@RequestParam String key) {
        redisService.delete(key);
        return "Deleted!";
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/vbs2nlwwr24hba1m>