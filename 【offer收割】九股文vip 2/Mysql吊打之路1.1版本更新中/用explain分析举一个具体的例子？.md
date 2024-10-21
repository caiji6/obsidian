# 用explain分析举一个具体的例子？

# 题目详细答案
假设我们有一个电子商务数据库，包含以下两个表：

1. orders表：

```sql
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT,
  order_date DATE,
  total_amount DECIMAL(10, 2)
);
```

2. customers表：

```sql
CREATE TABLE customers (
  customer_id INT PRIMARY KEY,
  customer_name VARCHAR(100),
  email VARCHAR(100)
);
```

我们有一个查询，想要获取在特定日期范围内下单的客户信息：

```sql
SELECT c.customer_name, c.email, o.order_date, o.total_amount FROM orders o JOIN customers c ON o.customer_id = c.customer_id WHERE o.order_date BETWEEN '2024-01-01' AND '2024-01-31';
```

## 使用 EXPLAIN 分析查询
执行EXPLAIN命令来分析这个查询：

```sql
EXPLAIN SELECT c.customer_name, c.email, o.order_date, o.total_amount FROM orders o JOIN customers c ON o.customer_id = c.customer_id WHERE o.order_date BETWEEN '2024-01-01' AND '2024-01-31';
```

## EXPLAIN 输出示例
假设EXPLAIN返回以下结果：

| id | select_type | table | partitions | type | possible_keys | key | key_len | ref | rows | filtered | Extra |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | SIMPLE | o | NULL | ALL | NULL | NULL | NULL | NULL | 10000 | 10.00 | Using where |
| 1 | SIMPLE | c | NULL | eq_ref | PRIMARY | PRIMARY | 4 | e_commerce.o.customer_id | 1 | 100.00 | NULL |


## 分析 EXPLAIN 输出
### orders 表扫描（o 表）
**type**: ALL表示全表扫描，这意味着 MySQL 需要扫描orders表的所有行。

**possible_keys**:NULL表示没有可用的索引。

**rows**: 10000 表示 MySQL 估计需要扫描 10,000 行。

**Extra**:Using where表示 MySQL 使用了WHERE子句来过滤行。

### customers 表扫描（c 表）
**type**:eq_ref表示对customer_id列的精确查找。

**key**:PRIMARY表示使用了customers表的主键索引。

**rows**: 1 表示每次查找只返回一行。

## 优化建议
**为orders表的order_date列创建索引**：

```sql
CREATE INDEX idx_order_date ON orders(order_date);
```

**再次运行 EXPLAIN**查看效果：

```sql
EXPLAIN SELECT c.customer_name, c.email, o.order_date, o.total_amount FROM orders o JOIN customers c ON o.customer_id = c.customer_id WHERE o.order_date BETWEEN '2024-01-01' AND '2024-01-31';
```

## 优化后的 EXPLAIN 输出示例
| id | select_type | table | partitions | type | possible_keys | key | key_len | ref | rows | filtered | Extra |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | SIMPLE | o | NULL | range | idx_order_date | idx_order_date | 3 | NULL | 1000 | 100.00 | Using index |
| 1 | SIMPLE | c | NULL | eq_ref | PRIMARY | PRIMARY | 4 | e_commerce.o.customer_id | 1 | 100.00 | NULL |


## 结果分析
### orders 表扫描（o 表）
**type**:range表示使用索引扫描特定范围的行。

**key**:idx_order_date表示使用了新创建的索引。

**rows**: 1000 表示 MySQL 估计需要扫描 1,000 行，而不是之前的 10,000 行。



通过创建索引，查询性能得到了显著提升，因为 MySQL 不再需要对orders表进行全表扫描，而是使用索引快速定位符合条件的行。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hxikhiwgihoie772>