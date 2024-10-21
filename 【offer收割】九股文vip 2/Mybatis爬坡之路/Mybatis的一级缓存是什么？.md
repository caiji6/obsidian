# 👌Mybatis的一级缓存是什么？

# 口语化回答
好的，面试官，<font style="color:rgb(36, 41, 47);">MyBatis 的一级缓存是用来减少数据库查询次数的，每个 SqlSession 都有自己的缓存。这个缓存默认是开启的，不需要额外配置。缓存只在同一个 SqlSession 内有效，不同的 SqlSession 之间互不影响。执行增删改操作、调用 commit() 或 rollback() 方法，或者关闭 SqlSession 时，缓存会被清空。当你执行查询操作时，MyBatis 会先看看缓存里有没有数据。如果有，就直接用缓存里的数据，不用再去查数据库。如果没有，就去查数据库，然后把结果放进缓存里。MyBatis 用查询的 SQL 语句和参数作为缓存的键。以上。</font>

# 题目详细答案
MyBatis 的一级缓存是一个本地缓存机制，用于减少数据库查询次数，提高系统性能。一级缓存是 SqlSession 级别的缓存，即每个 SqlSession 都有一个独立的缓存区域。

## 一级缓存特点
**作用范围**：一级缓存的作用范围是 SqlSession 级别的。同一个 SqlSession 对象在执行查询操作时，如果查询的数据已经存在于一级缓存中，则直接从缓存中获取，而不再执行数据库查询。

**默认开启**：一级缓存是默认开启的，无需额外配置。

**缓存范围**：一级缓存只对同一个 SqlSession 有效，不同的 SqlSession 之间的缓存数据互不影响。

**缓存失效**：在以下几种情况下，一级缓存会被清空：

1、 执行insert、update或delete操作后，一级缓存会被清空。

2、 调用SqlSession的commit()方法或rollback()方法后，一级缓存会被清空。

3、 关闭当前的SqlSession。

## 一级缓存的工作原理
当执行查询操作时，MyBatis 会先检查一级缓存中是否有相应的数据。如果有，则直接返回缓存中的数据；如果没有，则执行数据库查询，并将查询结果放入一级缓存中。MyBatis 使用查询的 SQL 语句和参数作为缓存的键（CacheKey），以确保缓存数据的唯一性和正确性。

## 示例 Demo
```plain
public class MyBatisCacheExample {
    public static void main(String[] args) {
        String resource="mybatis-config.xml";
        InputStream inputStream= Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory=new SqlSessionFactoryBuilder().build(inputStream);

        try (SqlSessionsession= sqlSessionFactory.openSession()) {
            UserMapper mapper= session.getMapper(UserMapper.class);

            // 第一次查询，数据将从数据库中获取，并放入一级缓存
            Useruser1= mapper.selectUserById(1);
            System.out.println(user1);

            // 第二次查询，数据将从一级缓存中获取
            Useruser2= mapper.selectUserById(1);
            System.out.println(user2);

            // 比较两次查询结果是否是同一个对象
            System.out.println(user1 == user2); // 输出 true
        }
    }
}
```

在上述代码中，第一次查询时，MyBatis 会从数据库中获取数据并放入一级缓存。第二次查询时，由于查询条件相同，MyBatis 会直接从一级缓存中获取数据，而不再执行数据库查询。

## 一级缓存的配置
虽然一级缓存默认是开启的，但可以通过配置文件进行一些控制，比如禁用一级缓存：

```plain
<settings>
    <settingname="localCacheScope"value="STATEMENT"/>、
</settings>
```

在上述配置中，将localCacheScope设置为STATEMENT，表示每次执行 SQL 语句后，一级缓存都会被清空，从而禁用一级缓存。默认情况下，localCacheScope的值是SESSION，表示一级缓存的作用范围是 SqlSession。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/svlmmq6n1glik36z>