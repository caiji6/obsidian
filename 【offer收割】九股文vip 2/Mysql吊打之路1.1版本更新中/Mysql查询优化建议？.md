# Mysql查询优化建议？

# 题目详细答案
## 避免全表扫描
优化查询时，应避免全表扫描，优先在 WHERE 和 ORDER BY 涉及的列上建立索引。

建立索引的方式看鸡哥前面的那道题，建立索引要考虑的因素有哪些？

## 避免 NULL 值判断
避免在 WHERE 子句中对字段进行 NULL 值判断。创建表时，尽量使用 NOT NULL 或使用特殊值（如 0 或 -1）作为默认值。

> Mysql难以优化引用可空列查询，它会使索引、索引统计和值更加复杂。可空列需要更多的存储空间，还需要mysql内部进行特殊处理。可空列被索引后，每条记录都需要一个额外的字节，还能导致MYisam 中固定大小的索引变成可变大小的索引。
>
> —— 出自《高性能mysql第二版》
>

还有几点原因：

1、所有使用NULL值的情况，都可以通过一个有意义的值的表示，这样有利于代码的可读性和可维护性，并能从约束上增强业务数据的规范性。

2、NULL值到非NULL的更新无法做到原地更新，更容易发生索引分裂，从而影响性能。

3、NOT IN、!= 等负向条件查询在有 NULL 值的情况下返回永远为空结果，查询容易出错

## 避免 != 或 <> 操作符
避免在 WHERE 子句中使用 != 或 <> 操作符。MySQL 仅对 <，<=，=，>，>=，BETWEEN，IN 以及某些情况下的 LIKE 使用索引。

## 避免 OR 条件
避免在 WHERE 子句中使用 OR 连接条件，因这会导致全表扫描。可以使用 UNION 合并查询：

```sql
SELECT id FROM t WHERE num=10 UNION ALL SELECT id FROM t WHERE num=20;
```

## 谨慎使用 IN 和 NOT IN
谨慎使用 IN 和 NOT IN，因其可能导致全表扫描。对于连续数值，用 BETWEEN 替代 IN：

```sql
SELECT id FROM t WHERE num BETWEEN 1 AND 3;
```

## LIKE 查询优化
避免使用%abc%或%abc的 LIKE 查询，这会导致全表扫描。可以考虑使用全文检索。只有abc%的 LIKE 查询会使用索引。

## 避免参数化查询导致的全表扫描
避免在 WHERE 子句中使用参数，这会导致全表扫描。可以强制查询使用索引：

```sql
SELECT id FROM t WITH (INDEX(索引名)) WHERE num=@num;
```

## 避免表达式操作
避免在 WHERE 子句中对字段进行函数操作。

不要`WHERE YEAR(order_date) = 2024`使用 ，而要使用

```sql
WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01'
```

## 使用 EXISTS 替代 IN
使用 EXISTS 替代 IN，可以提高查询效率：

```sql
SELECT num FROM a WHERE EXISTS (SELECT 1 FROM b WHERE num=a.num);
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/pdfiltfh2h5mpf1v>