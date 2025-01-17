# 👌mysql的行锁和表锁是什么？分别在哪些情况下会出现 ？

### 行锁（Row Lock）
**定义**： 行锁是指对数据库表中的某一行记录进行锁定。在行锁的情况下，不同事务可以并发地操作同一张表中的不同行记录，从而提高了并发性能。

**特点**：

+ **粒度小**：锁定的范围是具体的行记录，粒度较小，锁冲突的概率低，适合高并发操作。
+ **并发性高**：由于锁定的是具体的行记录，不同事务可以同时操作同一张表中的不同行，从而提高了并发性能。

**出现情况**： 行锁通常在以下情况下会出现：

1. **使用 InnoDB 存储引擎**：InnoDB 是 MySQL 默认的存储引擎，它支持行级锁。
2. **执行带有**`**WHERE**`**子句的**`**UPDATE**`**或**`**DELETE**`**语句**：如`UPDATE table SET column = value WHERE condition`。
3. **执行带有**`**WHERE**`**子句的**`**SELECT ... FOR UPDATE**`**或**`**SELECT ... LOCK IN SHARE MODE**`**语句**：这些语句会对查询到的行记录加锁。

**示例**：

```plain
-- 对满足条件的行加排他锁
SELECT * FROM employees WHERE id = 1 FOR UPDATE;

-- 对满足条件的行加共享锁
SELECT * FROM employees WHERE id = 1 LOCK IN SHARE MODE;
```

### 表锁（Table Lock）
**定义**： 表锁是对整个表进行锁定。在表锁的情况下，一个事务对表进行锁定后，其他事务不能对该表进行任何操作，直到锁被释放。

**特点**：

+ **粒度大**：锁定的范围是整个表，粒度较大，锁冲突的概率高，适合低并发操作。
+ **并发性低**：由于锁定的是整个表，在锁释放之前，其他事务无法对该表进行任何操作，从而降低了并发性能。

**出现情况**： 表锁通常在以下情况下会出现：

1. **使用 MyISAM 存储引擎**：MyISAM 存储引擎默认使用表级锁。
2. **执行**`**ALTER TABLE**`**、**`**DROP TABLE**`**、**`**TRUNCATE TABLE**`**等操作**：这些操作会对整个表加锁。
3. **显式请求表锁**：使用`LOCK TABLES`语句显式请求表锁。

**示例**：

```plain
-- 显式请求表锁
LOCK TABLES employees WRITE;

-- 释放表锁
UNLOCK TABLES;
```

### 总结
+ **行锁**：粒度小、并发性高，适用于高并发环境。通常在使用 InnoDB 存储引擎时，通过`UPDATE`、`DELETE`、`SELECT ... FOR UPDATE`等语句触发。
+ **表锁**：粒度大、并发性低，适用于低并发环境。通常在使用 MyISAM 存储引擎或执行`ALTER TABLE`、`DROP TABLE`等操作时触发，也可以通过`LOCK TABLES`语句显式请求。

选择合适的锁机制取决于具体的应用场景和性能需求。在高并发环境下，通常推荐使用支持行级锁的 InnoDB 存储引擎，以提高并发性能。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gnmbg4w2k7gq78b7>