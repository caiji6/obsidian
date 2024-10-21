# drop,delete与truncate的区别？

# 题目详细答案
## DROP
DROP用于删除数据库对象，如表、视图、索引等。可以删除表、数据库、视图、索引等。

```plain
DROP TABLE table_name;
DROP DATABASE database_name;
```

### 特点
完全删除表及其所有数据。删除表后，表的结构和数据都将永久消失，且无法恢复（除非有备份）。不会触发DELETE触发器。无法回滚（不可逆操作）。

## DELETE
DELETE用于删除表中符合条件的行。只能删除表中的数据行。

```plain
DELETE FROM table_name WHERE condition;
```

### 特点
可以通过WHERE子句指定删除哪些行，如果没有WHERE子句，将删除表中所有行，但表结构和索引保留。删除操作会记录在事务日志中，因此可以回滚（可逆操作）。可以触发DELETE触发器。相对较慢，因为每行删除操作都会记录在日志中，并且需要考虑事务处理。

## TRUNCATE
TRUNCATE用于快速删除表中的所有行。直接操作表中的所有数据行。

```plain
TRUNCATE TABLE table_name;
```

### 特点
删除所有行，但保留表结构及其列、索引等元数据。比DELETE快，因为它不逐行删除数据，而是直接释放数据页。无法回滚（不可逆操作），但在某些数据库系统中，TRUNCATE可能被视为DDL操作，具体行为取决于数据库系统的实现。重置自增列（AUTO_INCREMENT）计数器。

## 区别总结
| 操作对象 | 是否可回滚 | 性能 | 触发器 |
| --- | --- | --- | --- |
| DROP | 不可回滚 | 通常比DELETE快，因为它不逐行处理数据。 | 不会触发DELETE触发器 |
| DELETE | 可回滚 | 较慢，特别是当删除大量数据时，因为每行操作都会被记录在日志中。 | 会触发DELETE触发器 |
| TRUNCATE | 通常不可回滚（视数据库实现而定） | 通常比DELETE快，因为它不逐行处理数据。 | 不会触发DELETE触发器 |




> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ytwmryigauc87wwo>