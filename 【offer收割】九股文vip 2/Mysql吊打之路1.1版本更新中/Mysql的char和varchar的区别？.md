# 👌Mysql的char和varchar的区别？

在 MySQL 中，CHAR和VARCHAR是两种用于存储字符串的字段类型，区别如下：

### 1. 存储方式
+ **CHAR**：
    - CHAR是固定长度的字符串类型。
    - 无论实际存储的字符串长度是多少，CHAR类型的字段都会占用固定的空间。例如，CHAR(10)类型的字段，无论存储的字符串是 "abc" 还是 "abcdefghij"，都会占用 10 个字符的空间。
    - 如果存储的字符串长度小于定义的长度，MySQL 会在字符串的末尾填充空格以达到指定的长度。
+ **VARCHAR**：
    - VARCHAR是可变长度的字符串类型。
    - VARCHAR类型的字段根据实际存储的字符串长度来分配空间。例如，VARCHAR(10)类型的字段，存储 "abc" 只占用 3 个字符的空间（加上一个额外的字节用于存储字符串的长度）。
    - VARCHAR类型的字段在存储时会记录实际字符串的长度，因此不会有额外的空格填充。

### 2. 存储效率
+ **CHAR**：
    - 由于是固定长度，CHAR类型的字段在存储和检索时效率较高，特别适用于存储长度固定的字符串（如国家代码、邮政编码等）。
    - 但对于长度变化较大的字符串，CHAR类型可能会浪费大量的存储空间。
+ **VARCHAR**：
    - VARCHAR类型的字段在存储空间上更节省，因为它只分配实际需要的空间。
    - 对于长度变化较大的字符串，VARCHAR类型更加合适。

### 3. 性能
+ **CHAR**：
    - 由于固定长度，CHAR类型的字段在进行比较和检索时速度较快。
    - 适用于需要频繁查询和比较的字段。
+ **VARCHAR**：
    - VARCHAR类型的字段在存储和检索时需要额外的长度信息，因此在某些情况下性能可能稍逊于CHAR。
    - 适用于长度不固定且不需要频繁比较的字段。

### 4. 使用场景
+ **CHAR**：
    - 适用于存储长度固定的字符串，如固定长度的编码、标识符等。
    - 例如，存储国家代码（如 "USA"、"CHN"）或邮政编码（如 "12345"）。
+ **VARCHAR**：
    - 适用于存储长度可变的字符串，如姓名、地址、描述等。
    - 例如，存储用户的姓名、电子邮件地址或文章内容。

### 示例
```plain
CREATE TABLE example (
    fixed_length CHAR(10),
    variable_length VARCHAR(10)
);

INSERT INTO example (fixed_length, variable_length) VALUES ('abc', 'abc');

SELECT fixed_length, variable_length FROM example;
```

在上面的例子中，fixed_length字段会存储为 "abc "（填充空格到 10 个字符），而variable_length字段会存储为 "abc"（仅占用 3 个字符的空间）。

### 总结
+ CHAR类型适用于存储长度固定的字符串，具有较高的存储和比较效率，但可能会浪费存储空间。
+ VARCHAR类型适用于存储长度可变的字符串，能更有效地利用存储空间，但在某些情况下性能可能稍逊于CHAR。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/cr6co35a349c8h31>