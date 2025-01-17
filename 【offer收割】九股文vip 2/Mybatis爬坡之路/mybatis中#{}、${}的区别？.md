# 👌mybatis 中#{}、${}的区别？

# 口语化答案
好的，面试官。在 MyBatis 中，#{} 和 ${} 是用于参数绑定的两种不同的方式，在安全性和处理机制上有显著区别。在大多数情况下，应该优先使用 #{}，因为它更安全、更方便。当需要动态构建表名、列名或其他 SQL 语句的片段时，可以使用 ${}，尽量避免使用用户输入或其他不可信的数据作为参数。以上。

# 题目解析
高频题，主要就是想考二者的区别，以及谁是安全的，谁是容易 sql 注入问题的。

# 面试得分点
预编译，防 sql 注入，不是直接拼接，占位符

# 题目详细答案
## #{}（预编译方式）
安全性：#{} 采用了预编译（PreparedStatement）的方式，有效防止了 SQL 注入。因为 MyBatis 会将 #{} 中的内容当作参数处理，而不是直接拼接到 SQL 语句中。



处理方式：MyBatis 会为 #{} 中的内容生成一个参数占位符 ?，并使用 PreparedStatement 的 setXXX() 方法来设置参数值。因此，你不需要担心数据类型或引号问题。



例子：SELECT * FROM users WHERE id = #{userId}，这条 SQL 语句在 MyBatis 中处理时，会将其转换为类似 SELECT * FROM users WHERE id = ? 的形式，并通过 PreparedStatement 的 setInt() 或其他相关方法来设置 userId 的值。

## ${}（字符串拼接方式）
安全性：${} 是直接字符串拼接的方式，所以存在 SQL 注入的风险。如果参数来自用户输入或其他不可信的来源，那么使用 ${} 是非常危险的。



处理方式：MyBatis 会直接将 ${} 中的内容替换到 SQL 语句中。这意味着你需要自己处理数据类型、引号等问题。



用途：虽然 ${} 存在安全风险，但在某些场景下它是必要的。例如，当你要动态地构建表名或列名时，就必须使用 ${}。



例子：SELECT * FROM ${tableName}，这里的 tableName 是一个变量，MyBatis 会直接将其替换到 SQL 语句中。因此，如果你不能保证 tableName 的来源是可信的，那么这条 SQL 语句就存在 SQL 注入的风险。





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/khqhvodekrtb2wgx>