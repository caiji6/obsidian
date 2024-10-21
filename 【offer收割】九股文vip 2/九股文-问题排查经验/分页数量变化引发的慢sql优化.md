# 👌分页数量变化引发的慢sql优化

[此处为语雀卡片，点击链接查看](https://www.yuque.com/jingdianjichi/xyxdsi/gu01wef2zyy43o9o#cXDGZ)

鸡翅老哥今天发现一个很秀的问题，分页10条查询，和分页20条查询， SQL性能居然有2000倍的差距。是什么场景出现了这样的有趣的差别。

# 问题现象
在一个老项目后台进行功能改版过程中，通过监控发现了一个慢SQL，SQL 耗时高达 30秒，然而未改版之前的 SQL执行时长仅为 30毫秒，整体功能上并没有动这个 sql，为何会产生这种现象。

# 业务背景
业务背景上，没有什么特别的。就是根据用户的 id，来查询用户的列表数据。

数据表简化版：

数据库版本是 MySQL 5.5.38，库表结构如下，单表数据量1100万条。库表还有其它个字段，为便于阅读而省去

```plain
CREATETABLE `t_table` (
 `ID` bigint(20) NOTNULL AUTO_INCREMENT COMMENT '主键',
 `USER_ID` bigint(20) DEFAULTNULL COMMENT '用户 id',
 `EXT_ID` bigint(20) DEFAULTNULL COMMENT '额外信息 ID',
 `REGISTER_DATE` datetime DEFAULTNULL COMMENT '注册时间',
PRIMARY KEY (`ID`),
 KEY `idx_register_date` (`REGISTER_DATE`) USING BTREE,
 KEY `idx_userid_extid` (`USER_ID`,`EXT_ID`),
) ENGINE=InnoDB AUTO_INCREMENT=1DEFAULT CHARSET=utf8 COMMENT='某某业务表'
```

慢SQL的语句背景：

注意：WHERE条件中，USER_ID是一定会出现的。

```plain
SELECT * FROM T_TABLE
WHERE USER_ID = ?
ORDERBY REGISTER_DATE DESC limit ?, ?
```

最终查出引起性能变化的原因是，旧版本每页展示20条，新版本每页展示10条，即limit语句的参数不同，结果却导致了性能天差地别。

30ms limit 0,20

30000ms. limit 0,10

# 排查分析
收到慢SQL报警后，通过EXPLAIN命令执行检查慢SQL

```plain
EXPLAIN SELECT * FROM T_TABLE
WHERE USER_ID = ?
ORDERBY REGISTER_DATE DESC limit 0,10
```



MySQL优化器选择了错误的索引，即选择索引idx_register_date，其type为“index”。众所周知，“index”意味着全索引扫描，是否会执行全表扫描，要看查询的字段是否能被索引覆盖（覆盖索引）。 由于我们select的字段较多，idx_register_date索引上没有我们需要的全部字段，MySQL会按照索引的顺序查找数据行，执行全表扫描，由于表数据量过千万，SQL实际执行了 30秒。

# 初步判断
我们已经可以判断MySQL优化器选择了错误的索引，为了进一步证实，我们通过强制索引的方式做个验证，语句如下：

```plain
EXPLAIN SELECT * FROM T_TABLE
FORCE INDEX(idx_userid_extid）
WHERE USER_ID = ?
ORDERBY REGISTER_DATE DESC limit 0,10
```

其中Select语句执行速度为20毫秒，EXPLAIN的结果如下，可见结果中的type变成了“ref”，意味着性能要好很多，因为ref代表着匹配少量的行。



至此，我们已经确认了MySQL选择了错误的索引，我们可以删除idx_register_date索引，或者新增一个idx_userid_registerDate索引，使SQL一定能走到USER_ID开头的索引中。

# 深层分析
为什么旧版页面快，新版页面在特定的查询条件下变慢了呢？

MySQL在执行语句之前，并不能准确地知道满足条件的记录有多少条，只能通过统计信息来估算记录数，在本案例的场景下，MySQL可能认为idx_register_date更有区分度，且预扫描的行数确实也更少，导致了其选择了错误的索引。

如果一个查询语句使用了ORDER BY和LIMIT子句，当优化器发现使用一个索引会加速查询执行的时候，会尝试使用它。

MySQL8.0以后可以通过以下命令关闭这个特性 SET optimizer_switch = "prefer_ordering_index=off"; 但是在8.0以前的版本，这个是写死的。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gu01wef2zyy43o9o>