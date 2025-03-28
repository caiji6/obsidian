# mysql的覆盖索引是什么？

# 题目详细答案
之前给大家说回表查询的时候，提到过覆盖索引，覆盖索引是优化回表查询的好手段。

在 MySQL 中，覆盖索引（Covering Index）是指一个索引包含了查询所需的所有列，从而使查询可以完全从索引中获取数据，而不需要访问实际的数据行。这种技术可以显著提高查询性能，因为它减少了对表数据的访问次数。

想一下，如果我的非聚集索引，在建立的时候，有一个 name 列，而我们查询的时候，也是 select name 这样就在非聚集索引就可以查到。那就不用回表了。

## 索引覆盖的好处
1. **减少I/O操作**：查询可以直接从索引中读取所需的数据，避免了访问磁盘上的数据行，从而减少了I/O操作。
2. **提高查询性能**：由于减少了对数据行的访问，查询速度会更快。
3. **减少回表操作**：避免了回表操作，因为所有需要的数据都在索引中。

## 示例 Demo
假设有一个表employees，并且我们在last_name列上创建了一个非聚簇索引：

```sql
CREATE TABLE employees (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  hire_date DATE
) ENGINE=InnoDB;

CREATE INDEX idx_last_name ON employees(last_name);
```

现在我们执行一个查询：

```sql
SELECT first_name, hire_date FROM employees WHERE last_name = 'Smith';
```

在这种情况下，idx_last_name索引只包含last_name列的信息，因此 MySQL 需要通过索引找到满足条件的行指针（例如主键值），然后回表访问实际的数据行以获取first_name和hire_date列的数据。

### 使用覆盖索引
为了避免回表操作，可以创建一个覆盖索引，包含所有需要的列：

```sql
CREATE INDEX idx_last_name_full ON employees(last_name, first_name, hire_date);
```

现在执行相同的查询：

```sql
SELECT first_name, hire_date FROM employees WHERE last_name = 'Smith';
```

### 覆盖索引的工作原理
1. **索引查找**：MySQL 使用idx_last_name_full索引查找last_name为 'Smith' 的索引项。
2. **直接获取数据**：因为索引已经包含了first_name和hire_date列，MySQL 可以直接从索引中获取所有需要的数据，而不需要回表。

## 适用场景
覆盖索引特别适用于那些经常需要查询少量列而这些列可以包含在一个索引中的场景。例如：

查询经常使用的列，需要高查询性能的读密集型应用。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/rtg917a2vueyth4g>