# 👌mysql的redolog是什么？

MySQL 的重做日志（redo log）是 InnoDB 存储引擎中的一个关键组件，用于保证数据的持久性和一致性。重做日志记录了所有对数据库进行的修改操作，这些操作在实际写入数据文件之前先写入重做日志。

### 重做日志的作用
1. **数据恢复**：
    - 在数据库崩溃或意外关机时，重做日志可以用于恢复未完成的事务。通过重做日志，InnoDB 可以在重新启动时重新应用未完成的事务，确保数据的一致性。
2. **提高性能**：
    - 重做日志的存在使得 InnoDB 可以将事务提交的更改快速写入日志文件，而不是立即写入数据文件。这种机制减少了磁盘 I/O 操作，提高了数据库的性能。

### 重做日志的工作原理
1. **写入重做日志**：
    - 当一个事务开始时，所有的修改操作都会首先记录到内存中的重做日志缓存（redo log buffer）。
    - 当事务提交时，重做日志缓存中的内容会被刷新到磁盘上的重做日志文件中，以确保事务的持久性。
2. **重做日志应用**：
    - 在数据库重启时，InnoDB 会检查重做日志文件中的内容，并重新应用所有未完成的事务。这一过程称为“重做（redo）”。

### 重做日志的组成
重做日志由一组固定大小的日志文件组成，这些文件循环使用。具体配置项包括：

1. **重做日志文件路径和大小**：
    - 可以在 MySQL 配置文件（`my.cnf`或`my.ini`）中设置重做日志文件的路径和大小：

```plain
[mysqld]
innodb_log_group_home_dir = /var/lib/mysql/  # 重做日志文件的存储路径
innodb_log_file_size = 512M  # 每个重做日志文件的大小
innodb_log_files_in_group = 2  # 重做日志文件的数量
```

1. **重做日志缓冲区大小**：
    - 设置重做日志缓冲区的大小：

```plain
[mysqld]
innodb_log_buffer_size = 16M
```

### 管理重做日志
1. **查看重做日志状态**：
    - 可以通过以下命令查看重做日志的状态：

```plain
SHOW ENGINE INNODB STATUS;
```

1. **调整重做日志配置**：
    - 调整重做日志文件的大小和数量需要先关闭 MySQL 服务，删除现有的重做日志文件，然后重新启动 MySQL 服务。具体步骤如下：
        * 关闭 MySQL 服务：

```plain
sudo systemctl stop mysql
```

+ 删除现有的重做日志文件（通常位于数据目录中，例如`/var/lib/mysql`）：

```plain
sudo rm /var/lib/mysql/ib_logfile*
```

+ 修改 MySQL 配置文件中的重做日志设置。
+ 重新启动 MySQL 服务：

```plain
sudo systemctl start mysql
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/km5rac1251nm0m3f>