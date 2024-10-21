# 👌Mybatis自带连接池都有什么？

# 口语化回答
好的，面试官，mybatsi 自带链接池主要有两种。`PooledDataSource`和`UnpooledDataSource`。`UnpooledDataSource`是一个未池化的连接池实现。每次请求数据库连接时，`UnpooledDataSource`都会创建一个新的连接，而不是从连接池中获取连接。适用于对性能要求不高的小型应用或测试环境。`PooledDataSource`则维护了一组数据库连接，以便在需要时快速提供连接，而不需要每次都创建新的连接。`PooledDataSource`提供了基本的连接池功能，如连接池大小、连接超时等。以上。

# 面试得分点
`PooledDataSource`，`UnpooledDataSource`，数据连接获取

# 题目详细答案
MyBatis 自带的连接池实现主要有两种：`PooledDataSource`和`UnpooledDataSource`。这两种连接池实现提供了不同的特性和使用场景。

## `UnpooledDataSource`
`UnpooledDataSource`是 MyBatis 提供的一个简单的、未池化的连接池实现。每次请求数据库连接时，`UnpooledDataSource`都会创建一个新的连接，而不是从连接池中获取连接。这种实现适用于对性能要求不高的小型应用或测试环境。

```plain
<environment id="development">
    <transactionManager type="JDBC"/>
    <dataSource type="UNPOOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mydatabase"/>
        <property name="username" value="root"/>
        <property name="password" value="password"/>
    </dataSource>
</environment>
```

## `PooledDataSource`
`PooledDataSource`是 MyBatis 提供的一个简单的连接池实现。它维护了一组数据库连接，以便在需要时快速提供连接，而不需要每次都创建新的连接。`PooledDataSource`提供了基本的连接池功能，如连接池大小、连接超时等。

```plain
<environment id="development">
    <transactionManager type="JDBC"/>
    <dataSource type="POOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mydatabase"/>
        <property name="username" value="root"/>
        <property name="password" value="password"/>
        <property name="poolMaximumActiveConnections" value="10"/>
        <property name="poolMaximumIdleConnections" value="5"/>
        <property name="poolMaximumCheckoutTime" value="20000"/>
        <property name="poolTimeToWait" value="20000"/>
    </dataSource>
</environment>
```

### `PooledDataSource`配置属性：
`poolMaximumActiveConnections`: 连接池中最大活跃连接数。

`poolMaximumIdleConnections`: 连接池中最大空闲连接数。

`poolMaximumCheckoutTime`: 连接被检查出并返回之前的最长时间（以毫秒为单位）。

`poolTimeToWait`: 在无法获取连接时，线程在重新尝试之前等待的时间（以毫秒为单位）。

## 选择连接池实现
虽然 MyBatis 提供了内置的连接池实现，但在生产环境中，通常推荐使用功能更强大、性能更好的第三方连接池，例如：

**HikariCP**：以高性能和低延迟著称。

**Druid**：一个成熟且广泛使用的连接池实现。

**C3P0**：一个功能丰富的连接池实现，适用于需要高级功能的场景。

使用第三方连接池时，可以通过配置 MyBatis 使用外部的数据源来替代内置的连接池。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gdeeg33ztxssg9yl>