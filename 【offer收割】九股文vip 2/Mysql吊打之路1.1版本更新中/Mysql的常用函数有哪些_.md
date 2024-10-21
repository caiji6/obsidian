# 👌Mysql的常用函数有哪些?

### 字符串函数
1. **CONCAT(str1, str2, ...)**：连接多个字符串。

```plain
SELECT CONCAT('Hello', ' ', 'World');
```

1. **SUBSTRING(str, pos, len)**：从字符串str的pos位置开始，截取长度为len的子字符串。

```plain
SELECT SUBSTRING('Hello World', 7, 5);
```

1. **LENGTH(str)**：返回字符串的长度（字节数）。

```plain
SELECT LENGTH('Hello');
```

1. **UPPER(str)**：将字符串转换为大写。

```plain
SELECT UPPER('hello');
```

1. **LOWER(str)**：将字符串转换为小写。

```plain
SELECT LOWER('HELLO');
```

1. **TRIM(str)**：去除字符串两端的空格。

```plain
SELECT TRIM('  Hello World  ');
```

1. **REPLACE(str, from_str, to_str)**：将字符串str中的from_str替换为to_str。

```plain
SELECT REPLACE('Hello World', 'World', 'MySQL');
```

### 数值函数
1. **ABS(x)**：返回x的绝对值。

```plain
SELECT ABS(-10);
```

1. **CEIL(x)**或**CEILING(x)**：返回大于或等于x的最小整数。

```plain
SELECT CEIL(4.2);
```

1. **FLOOR(x)**：返回小于或等于x的最大整数。

```plain
SELECT FLOOR(4.8);
```

1. **ROUND(x, d)**：将x四舍五入到d位小数。

```plain
SELECT ROUND(123.456, 2);
```

1. **RAND()**：返回一个 0 到 1 之间的随机数。

```plain
SELECT RAND();
```

1. **POWER(x, y)**或**POW(x, y)**：返回x的y次幂。

```plain
SELECT POWER(2, 3);
```

### 日期和时间函数
1. **NOW()**：返回当前日期和时间。

```plain
SELECT NOW();
```

1. **CURDATE()**或**CURRENT_DATE()**：返回当前日期。

```plain
SELECT CURDATE();
```

1. **CURTIME()**或**CURRENT_TIME()**：返回当前时间。

```plain
SELECT CURTIME();
```

1. **DATE_FORMAT(date, format)**：格式化日期。

```plain
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');
```

1. **DATEDIFF(date1, date2)**：返回两个日期之间的天数差。

```plain
SELECT DATEDIFF('2024-12-31', '2024-01-01');
```

1. **ADDDATE(date, interval)**或**DATE_ADD(date, interval)**：在日期上加上一个时间间隔。

```plain
SELECT ADDDATE('2024-01-01', INTERVAL10DAY);
```

1. **SUBDATE(date, interval)**或**DATE_SUB(date, interval)**：在日期上减去一个时间间隔。

```plain
SELECT SUBDATE('2024-01-11', INTERVAL10DAY);
```

### 聚合函数
1. **COUNT(expression)**：返回满足条件的行数。

```plain
SELECT COUNT(*) FROM table_name;
```

1. **SUM(expression)**：返回数值列的总和。

```plain
SELECT SUM(column_name) FROM table_name;
```

1. **AVG(expression)**：返回数值列的平均值。

```plain
SELECT AVG(column_name) FROM table_name;
```

1. **MAX(expression)**：返回列的最大值。

```plain
SELECT MAX(column_name) FROM table_name;
```

1. **MIN(expression)**：返回列的最小值。

```plain
SELECT MIN(column_name) FROM table_name;
```

### 其他常用函数
1. **IF(condition, true_value, false_value)**：条件判断函数。

```plain
SELECT IF(1>0, 'true', 'false');
```

1. **COALESCE(value1, value2, ...)**：返回第一个非 NULL 的值。

```plain
SELECT COALESCE(NULL, NULL, 'MySQL');
```

1. **IFNULL(expression, alt_value)**：如果expression为 NULL，返回alt_value。

```plain
SELECT IFNULL(NULL, 'default');
```

1. **NULLIF(expr1, expr2)**：如果expr1等于expr2，返回 NULL，否则返回expr1。

```plain
SELECT NULLIF(1, 1);
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fmutvu4avivnkx4r>