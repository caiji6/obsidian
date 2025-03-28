# 👌mysql的binlog是什么？

MySQL 的二进制日志（binlog）是一个重要的日志文件，用于记录对数据库进行的所有更改操作。二进制日志的主要功能包括数据恢复、复制和审计。

### 二进制日志的作用
1. **数据恢复**：
    - 在发生崩溃或数据丢失时，可以使用二进制日志恢复数据。通过重放日志中的更改，可以将数据库恢复到某个时间点。
2. **复制**：
    - 二进制日志是 MySQL 复制机制的基础。主服务器（master）将其上的所有更改记录到二进制日志中，从服务器（slave）通过读取这些日志来复制数据变化，从而保持数据同步。
3. **审计和分析**：
    - 二进制日志可以用来审计数据库上的更改操作，帮助管理员了解谁在何时对数据库进行了哪些更改。

### 二进制日志的组成
二进制日志由多个日志文件组成，这些文件按照顺序记录了所有的更改操作。每个日志文件都有一个唯一的编号，MySQL 会自动轮换和生成新的日志文件。

### 配置和管理二进制日志
1. **启用二进制日志**：
    - 在 MySQL 配置文件（通常是`my.cnf`或`my.ini`）中，添加以下配置项以启用二进制日志：

```plain
[mysqld]
log-bin=mysql-bin
```

1. **查看二进制日志状态**：
    - 使用以下 SQL 命令查看二进制日志的状态：

```plain
SHOW BINARY LOGS;
```

+ 查看当前正在使用的二进制日志文件：

```plain
SHOW MASTER STATUS;
```

1. **管理二进制日志**：
    - 手动刷新二进制日志文件：

```plain
FLUSH LOGS;
```

+ 删除旧的二进制日志文件：或者：

```plain
PURGE BINARY LOGS TO 'mysql-bin.000010';
```

```plain
PURGE BINARY LOGS BEFORE '2024-01-01 00:00:00';
```

1. **配置二进制日志保留策略**：
    - 可以在配置文件中设置保留策略，例如自动删除7天前的二进制日志：

```plain
[mysqld]
expire_logs_days = 7
```

### 解析二进制日志
MySQL 提供了`mysqlbinlog`工具，用于读取和解析二进制日志文件。以下是一些常用的命令：

1. **显示二进制日志内容**：

```plain
mysqlbinlog mysql-bin.000001
```

1. **将二进制日志内容导入数据库**：

```plain
mysqlbinlog mysql-bin.000001 | mysql -u username -p
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/oyo55gmhgchwyg0i>