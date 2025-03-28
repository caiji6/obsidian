# 👌MySQL 有关权限的表都有哪几个？

1. **user**表
    - **用途**：存储全局级别的权限（适用于所有数据库）的用户信息。
    - **字段**：包括用户名、主机名、密码（已加密）以及各种全局权限（如SELECT,INSERT,UPDATE,DELETE, 等）。
2. **db**表
    - **用途**：存储数据库级别的权限。
    - **字段**：包括用户名、主机名、数据库名以及各种数据库级权限。
3. **tables_priv**表
    - **用途**：存储表级别的权限。
    - **字段**：包括用户名、主机名、数据库名、表名以及各种表级权限（如SELECT,INSERT,UPDATE,DELETE, 等）。
4. **columns_priv**表
    - **用途**：存储列级别的权限。
    - **字段**：包括用户名、主机名、数据库名、表名、列名以及各种列级权限（如SELECT,INSERT,UPDATE, 等）。
5. **procs_priv**表
    - **用途**：存储存储过程和存储函数级别的权限。
    - **字段**：包括用户名、主机名、数据库名、存储过程或存储函数名以及各种权限（如EXECUTE,ALTER ROUTINE, 等）。
6. **proxies_priv**表
    - **用途**：存储代理用户权限。
    - **字段**：包括用户名、主机名、被代理用户名、被代理主机名以及代理权限。

一些示例查询，用于查看这些权限表中的内容：

```plain
-- 查看全局权限
SELECT * FROM mysql.user;
-- 查看数据库级别权限
SELECT * FROM mysql.db;
-- 查看表级别权限
SELECT * FROM mysql.tables_priv;
-- 查看列级别权限
SELECT * FROM mysql.columns_priv;
-- 查看存储过程和存储函数级别权限
SELECT * FROM mysql.procs_priv;
-- 查看代理用户权限
SELECT * FROM mysql.proxies_priv;
```

### 解释与注意事项
+ **全局权限**：存储在user表中，应用于所有数据库。
+ **数据库级别权限**：存储在db表中，应用于特定数据库中的所有表。
+ **表级别权限**：存储在tables_priv表中，应用于特定数据库中的特定表。
+ **列级别权限**：存储在columns_priv表中，应用于特定数据库中的特定表的特定列。
+ **存储过程和存储函数级别权限**：存储在procs_priv表中，应用于特定数据库中的特定存储过程或存储函数。
+ **代理用户权限**：存储在proxies_priv表中，允许一个用户代理另一个用户执行操作。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/tcb67ggttqom3woo>