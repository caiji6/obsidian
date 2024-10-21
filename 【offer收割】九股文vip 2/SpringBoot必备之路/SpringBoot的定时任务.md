# 👌SpringBoot的定时任务

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring Boot 中，定时任务可以使用</font>`<font style="background-color:rgb(247, 248, 249);">@Scheduled</font>`<font style="background-color:rgb(247, 248, 249);">注解来实现。</font>

<font style="background-color:rgb(247, 248, 249);">首先，你需要在主应用类或配置类上添加</font>`<font style="background-color:rgb(247, 248, 249);">@EnableScheduling</font>`<font style="background-color:rgb(247, 248, 249);">注解，以启用 Spring 的定时任务调度功能。</font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

### <font style="background-color:rgb(247, 248, 249);">步骤 2：创建定时任务</font>
<font style="background-color:rgb(247, 248, 249);">可以在任何 Spring 管理的 Bean 中使用</font>`<font style="background-color:rgb(247, 248, 249);">@Scheduled</font>`<font style="background-color:rgb(247, 248, 249);">注解来定义定时任务。</font>

#### <font style="background-color:rgb(247, 248, 249);">示例：</font>
```plain
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ScheduledTasks {

    // 每隔5秒执行一次
    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        System.out.println("Current Time: " + System.currentTimeMillis());
    }

    // 每隔5秒执行一次，上一次任务结束后再等待5秒
    @Scheduled(fixedDelay = 5000)
    public void reportCurrentTimeWithFixedDelay() {
        System.out.println("Current Time with Fixed Delay: " + System.currentTimeMillis());
    }

    // 第一次延迟1秒后执行，然后每隔5秒执行一次
    @Scheduled(initialDelay = 1000, fixedRate = 5000)
    public void reportCurrentTimeWithInitialDelay() {
        System.out.println("Current Time with Initial Delay: " + System.currentTimeMillis());
    }

    // 使用Cron表达式来定义任务执行时间
    @Scheduled(cron = "0 0 * * * ?")
    public void reportCurrentTimeWithCron() {
        System.out.println("Cron Scheduled Task: " + System.currentTimeMillis());
    }
}
```

### `<font style="background-color:rgb(247, 248, 249);">@Scheduled</font>`<font style="background-color:rgb(247, 248, 249);">参数说明</font>
+ `<font style="background-color:rgb(247, 248, 249);">fixedRate</font>`<font style="background-color:rgb(247, 248, 249);">: 以固定频率执行任务，单位为毫秒。任务开始后，间隔指定时间再次执行，不论上一次任务是否完成。</font>
+ `<font style="background-color:rgb(247, 248, 249);">fixedDelay</font>`<font style="background-color:rgb(247, 248, 249);">: 以固定延迟执行任务，单位为毫秒。任务完成后，间隔指定时间再次执行。</font>
+ `<font style="background-color:rgb(247, 248, 249);">initialDelay</font>`<font style="background-color:rgb(247, 248, 249);">: 第一次执行任务前的延迟时间，单位为毫秒。</font>
+ `<font style="background-color:rgb(247, 248, 249);">cron</font>`<font style="background-color:rgb(247, 248, 249);">: 使用 Cron 表达式来定义任务执行时间。</font>

### <font style="background-color:rgb(247, 248, 249);">Cron 表达式</font>
<font style="background-color:rgb(247, 248, 249);">Cron 表达式是一种特殊的字符串格式，用于指定任务的执行时间。它由六个或七个空格分隔的字段组成，每个字段代表一个时间单位。</font>

#### <font style="background-color:rgb(247, 248, 249);">Cron 表达式字段：</font>
```plain
秒 分 时 日 月 星期 [年]
```

#### <font style="background-color:rgb(247, 248, 249);">示例：</font>
+ `<font style="background-color:rgb(247, 248, 249);">"0 0 * * * ?"</font>`<font style="background-color:rgb(247, 248, 249);">: 每小时执行一次。</font>
+ `<font style="background-color:rgb(247, 248, 249);">"0 0 12 * * ?"</font>`<font style="background-color:rgb(247, 248, 249);">: 每天中午12点执行一次。</font>
+ `<font style="background-color:rgb(247, 248, 249);">"0 0 12 * * MON-FRI"</font>`<font style="background-color:rgb(247, 248, 249);">: 每周一到周五中午12点执行一次。</font>
+ `<font style="background-color:rgb(247, 248, 249);">"0 0/5 * * * ?"</font>`<font style="background-color:rgb(247, 248, 249);">: 每5分钟执行一次。</font>

### <font style="background-color:rgb(247, 248, 249);">完整示例</font>
<font style="background-color:rgb(247, 248, 249);"></font>

```plain
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@SpringBootApplication
@EnableScheduling
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}

@Component
class ScheduledTasks {

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        System.out.println("Current Time: " + System.currentTimeMillis());
    }

    @Scheduled(fixedDelay = 5000)
    public void reportCurrentTimeWithFixedDelay() {
        System.out.println("Current Time with Fixed Delay: " + System.currentTimeMillis());
    }

    @Scheduled(initialDelay = 1000, fixedRate = 5000)
    public void reportCurrentTimeWithInitialDelay() {
        System.out.println("Current Time with Initial Delay: " + System.currentTimeMillis());
    }

    @Scheduled(cron = "0 0 * * * ?")
    public void reportCurrentTimeWithCron() {
        System.out.println("Cron Scheduled Task: " + System.currentTimeMillis());
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/osd2d0bt3km1ptdx>