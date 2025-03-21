# 👌mysql什么情况下会产生死锁？

### 1. 不同顺序的锁定
当两个或多个事务以不同的顺序请求相同的资源时，容易引发死锁。

**示例**：

+ 事务A先锁定表1的记录，然后尝试锁定表2的记录。
+ 事务B先锁定表2的记录，然后尝试锁定表1的记录。
+ 如果事务A和事务B同时执行，就可能导致死锁。

### 2. 间隙锁（Gap Lock）
在InnoDB存储引擎中，间隙锁用于防止幻读。在范围查询中，间隙锁可能会导致死锁。

**示例**：

+ 事务A执行SELECT * FROM table WHERE id > 10 FOR UPDATE，锁定了id大于10的所有记录及其间隙。
+ 事务B执行INSERT INTO table (id) VALUES (15)，试图插入一个新记录。
+ 如果这两个事务并发执行，可能会导致死锁。

### 3. 自增列的死锁
在高并发情况下，当多个事务同时插入自增列时，可能会导致死锁。

**示例**：

+ 事务A和事务B同时插入数据到包含自增主键的表中。
+ MySQL在分配自增值时可能会导致锁争用，从而引发死锁。

### 4. 外键约束
在涉及外键约束的表中，更新或删除操作可能会导致死锁。

**示例**：

+ 事务A在表1中删除一条记录，该表有一个外键引用表2。
+ 事务B在表2中更新或删除与表1中记录相关的记录。
+ 如果这两个事务并发执行，可能会导致死锁。

### 5. 锁升级
当MySQL从行级锁升级到表级锁时，可能会导致死锁。

**示例**：

+ 事务A和事务B分别锁定同一表中的不同行。
+ 如果某个操作需要将行级锁升级为表级锁，这可能会导致死锁。

### 6. 混合使用不同类型的锁
在同一个事务中混合使用不同类型的锁（如读锁和写锁）时，可能会导致死锁。

**示例**：

+ 事务A持有一个读锁，并试图获取一个写锁。
+ 事务B持有一个写锁，并试图获取一个读锁。
+ 如果这两个事务并发执行，可能会导致死锁。

### 7. 大量并发事务
在高并发环境中，大量事务同时操作同一资源，可能会导致死锁。

**示例**：

+ 多个事务同时对同一行数据进行更新操作。
+ 事务之间相互等待资源释放，可能会导致死锁。

### 8. 长时间持有锁
当事务长时间持有锁，其他事务可能会在等待过程中产生死锁。

**示例**：

+ 事务A长时间持有某个记录的锁。
+ 事务B试图获取该记录的锁，但被阻塞。
+ 如果事务A和事务B都在等待对方释放锁，可能会导致死锁。

### 处理和预防死锁的方法
1. **自动检测和回滚**：InnoDB存储引擎能够自动检测死锁，并回滚其中一个事务以解除死锁。
2. **查看死锁信息**：使用命令SHOW ENGINE INNODB STATUS查看最近一次死锁的信息，以帮助诊断问题。
3. **合理的事务设计**：尽量避免长时间持有锁，确保事务尽可能短小和高效。
4. **一致的锁定顺序**：确保所有事务以相同的顺序请求资源，以减少死锁的可能性。
5. **减少并发事务**：通过优化应用程序逻辑，减少同时并发的事务数量。
6. **使用合适的隔离级别**：根据应用场景选择合适的事务隔离级别，以减少锁冲突。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ggh4gs30guupesdq>