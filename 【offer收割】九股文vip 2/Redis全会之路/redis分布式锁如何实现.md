# redis 分布式锁如何实现

聊到这个话题，redis分布式锁最好的理解方式就是进行循序渐进。接下来鸡哥带你们从头感受一下，分布式锁演进进程。

## 0.1版本
在之前的题目里，给大家说过两个命令。setnx，expire。二者组合实现一个最简单的锁。setnx成功，则证明抢到锁，然后使用expire设置一个过期时间，防止死锁。

```plain
//加锁
if（redisClient.setnx(lockKey,lockValue)）{
    //设置过期时间
    redisClient.expire（lockKey，1000）;
    try{
        //业务请求
        do something  
    }catch(){
    
    }finally{
       //释放锁
       redisClient.del(lockKey);
    }
}
```

那么问题来了。setnx 与 expire 不是原子的。假设刚执行完setnx，服务挂了，或重启，总之就是没执行expire，那么就变成死锁了。

## 0.2版本
针对0.1版本的原子性问题，很容易想到了lua脚本。

将setnx和expire写成lua命令，可以解决这个问题。那么还有没有基于api的简单方式呢。

set有一个扩展命令。这个命令是原子的。

SET key value[EX seconds][PX milliseconds][NX|XX]

于是可以这样做

```plain
//加锁
if（redisClient.set(lockKey,lockValue,"NX","EX", 1000)）{
    try{
        //业务请求
        do something  
    }catch(){
    
    }finally{
       //释放锁
       redisClient.del(lockKey);
    }
}
```

到这里，已经解决了加锁问题，那么释放锁是否有问题呢。

如果仅仅的使用lockKey删除，那是不是意味着多个线程的key一样，如果线程A没有执行完，锁过期了。线程B执行，加锁后，结果A执行完，把B的锁释放了。这就是A删错了人。

所以在此基础上要在释放锁在一个文章。增加唯一key标识的校验。

## 0.3 版本
```plain
//加锁
if（redisClient.set(lockKey,uuid,"NX","EX", 1000)）{
    try{
        //业务请求
        do something  
    }catch(){
    
    }finally{
       //释放锁
        if(uuid.equals(redisClient.get(lockKey);){
             redisClient.del(lockKey);
        }
      
    }
}
```

同样的释放锁，不是原子的，也可以使用lua脚本进行解决。

至此，0.3版本基本已经可以满足大多数场景的使用了。

那么假设。锁释放了，业务还没执行完怎么办。这个时候就使用之前给大家介绍的看门狗机制和reddisson框架即可。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/erpqpvttzv3tzrax>