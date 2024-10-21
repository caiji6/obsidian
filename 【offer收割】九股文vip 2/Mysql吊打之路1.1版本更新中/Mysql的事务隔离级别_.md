# Mysql的事务隔离级别?

# 口语化回答
好的，面试官。MySQL 主要支持四种事务隔离级别，分别是读未提交、读已提交、可重复读和串行化。

读未提交的意思是一个事务可以读取另一个未提交事务的数据，可能导致脏读，即一个事务读取了另一个事务未提交的数据。如果该事务回滚，那么读到的数据将是无效的。

读已提交是说一个事务只能读取已经提交的事务的数据。这样可以避免脏读，但可能会出现不可重复读，即A事务读取完数据后B事务提交数据，A事务再次读取的数据和上次不相同。

可重复读是说一个事务在整个过程中多次读取同一行数据时，结果是相同的。可以避免脏读和不可重复读，但可能会出现幻读。

串行化，这种级别下，事务完全串行化执行，避免了脏读、不可重复读和幻读。代价是并发性大大降低，事务可能会因为锁等待而阻塞。

# 题目解析
这道题主要考察对MySQL事务隔离级别的掌握能力，需要区分不同的事务隔离级别下可能会导致什么问题，并在实际业务中判断应当使用哪种隔离级别。

# 面试得分点
读未提交，读已提交，可重复读，串行化

# 题目详细答案
MySQL 支持四种事务隔离级别，每种隔离级别提供不同程度的数据一致性和并发性保障。

### 读未提交（Read Uncommitted）
在这种隔离级别下，一个事务可以读取另一个未提交事务的数据。这种级别提供最少的数据保护，可能导致脏读（Dirty Read）。

**脏读**：一个事务读取了另一个事务未提交的数据。如果该事务回滚，那么读到的数据将是无效的。

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

### 读已提交（Read Committed）
在这种隔离级别下，一个事务只能读取已经提交的事务的数据。这样可以避免脏读，但可能会出现不可重复读（Non-repeatable Read）。

**不可重复读**：一个事务在读取同一行数据时，可能因为另一个事务的提交而得到不同的结果。

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

### 可重复读（Repeatable Read）
在这种隔离级别下，一个事务在整个过程中多次读取同一行数据时，结果是相同的。这种级别可以避免脏读和不可重复读，但可能会出现幻读（Phantom Read）。

**幻读**：一个事务在读取某个范围内的行时，另一个事务在该范围内插入了新的行，导致前一个事务再次读取时发现了“幻影”行。

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### 串行化（Serializable）
这是最高的隔离级别，在这种级别下，事务完全串行化执行，避免了脏读、不可重复读和幻读。代价是并发性大大降低，事务可能会因为锁等待而阻塞。

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

设置 MySQL 事务隔离级别的示例：

```plain
-- 设置全局事务隔离级别为可重复读SETGLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
-- 设置当前会话的事务隔离级别为读已提交SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- 开始事务START TRANSACTION;
-- 在事务内进行操作SELECT*FROM accounts WHERE account_id ='A';
-- 提交事务COMMIT;
```

### 总结
**读未提交（Read Uncommitted）**：最低级别，可能导致脏读。

**读已提交（Read Committed）**：避免脏读，但可能出现不可重复读。

**可重复读（Repeatable Read）**：避免脏读和不可重复读，但可能出现幻读。MySQL InnoDB 存储引擎的默认隔离级别。

**串行化（Serializable）**：最高级别，避免所有并发问题，但性能最低。

选择合适的隔离级别需要权衡数据一致性和系统性能之间的关系。对于大多数应用，默认的可重复读隔离级别已经能够提供足够的数据一致性和并发性能。

| **隔离级别** | **脏读** | **不可重复读** | **幻读** | **原理** |
| --- | --- | --- | --- | --- |
| <font style="color:rgb(77, 77, 77);">Read uncommitted (读未提交)</font> | 存在 | 存在 | 存在 | 直接读取数据，不处理并发问题 |
| <font style="color:rgb(77, 77, 77);">Read committed (读已提交)</font> | × | 存在 | 存在 | 读操作不加锁，写操作加排他锁 |
| <font style="color:rgb(77, 77, 77);">Repeatable read (可重复读)</font> | × | × | 存在 | MVCC实现，事务开始时创建ReadView，之后事务的其他查询都用这个ReadView |
| <font style="color:rgb(77, 77, 77);">Serializable (串行化)</font> | × | × | × | 使用锁，读加共享锁，写加排他锁，串行执行 |


### MySQL默认隔离级别
InnoDB默认使用的是可重复读隔离级别。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ge31ov5wqg3cyzsm>