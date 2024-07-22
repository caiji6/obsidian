[![19、分页数量导致的s慢ql的优化.mp4 (13.28MB)](https://gw.alipayobjects.com/mdn/prod_resou/afts/img/A*NNs6TKOR3isAAAAAAAAAAABkARQnAQ)](https://www.yuque.com/docs/176645995?_lake_card=%7B%22status%22%3A%22done%22%2C%22name%22%3A%2219%E3%80%81%E5%88%86%E9%A1%B5%E6%95%B0%E9%87%8F%E5%AF%BC%E8%87%B4%E7%9A%84s%E6%85%A2ql%E7%9A%84%E4%BC%98%E5%8C%96.mp4%22%2C%22size%22%3A13927464%2C%22taskId%22%3A%22u072a663f-9972-4104-8322-bddf9d1b080%22%2C%22taskType%22%3A%22upload%22%2C%22url%22%3Anull%2C%22cover%22%3Anull%2C%22videoId%22%3A%22inputs%2Fprod%2Fyuque%2F2024%2F29413969%2Fmp4%2F1719845674401-05a7bdb8-484a-44a4-a7ea-62b4853c45d4.mp4%22%2C%22download%22%3Afalse%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22cXDGZ%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22video%22%7D#cXDGZ)鸡翅老哥今天发现一个很秀的问题，分页10条查询，和分页20条查询， SQL性能居然有2000倍的差距。是什么场景出现了这样的有趣的差别。
# 问题现象
在一个老项目后台进行功能改版过程中，通过监控发现了一个慢SQL，SQL 耗时高达 30秒，然而未改版之前的 SQL执行时长仅为 30毫秒，整体功能上并没有动这个 sql，为何会产生这种现象。
# 业务背景
业务背景上，没有什么特别的。就是根据用户的 id，来查询用户的列表数据。<br />数据表简化版：<br />数据库版本是 MySQL 5.5.38，库表结构如下，单表数据量1100万条。库表还有其它个字段，为便于阅读而省去
```
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
慢SQL的语句背景：<br />注意：WHERE条件中，USER_ID是一定会出现的。
```
SELECT * FROM T_TABLE
WHERE USER_ID = ?
ORDERBY REGISTER_DATE DESC limit ?, ?
```
最终查出引起性能变化的原因是，旧版本每页展示20条，新版本每页展示10条，即limit语句的参数不同，结果却导致了性能天差地别。<br />30ms limit 0,20<br />30000ms. limit 0,10
# 排查分析
收到慢SQL报警后，通过EXPLAIN命令执行检查慢SQL
```
EXPLAIN SELECT * FROM T_TABLE
WHERE USER_ID = ?
ORDERBY REGISTER_DATE DESC limit 0,10
```

MySQL优化器选择了错误的索引，即选择索引idx_register_date，其type为“index”。众所周知，“index”意味着全索引扫描，是否会执行全表扫描，要看查询的字段是否能被索引覆盖（覆盖索引）。 由于我们select的字段较多，idx_register_date索引上没有我们需要的全部字段，MySQL会按照索引的顺序查找数据行，执行全表扫描，由于表数据量过千万，SQL实际执行了 30秒。
# 初步判断
我们已经可以判断MySQL优化器选择了错误的索引，为了进一步证实，我们通过强制索引的方式做个验证，语句如下：
```
EXPLAIN SELECT * FROM T_TABLE
FORCE INDEX(idx_userid_extid）
WHERE USER_ID = ?
ORDERBY REGISTER_DATE DESC limit 0,10
```
其中Select语句执行速度为20毫秒，EXPLAIN的结果如下，可见结果中的type变成了“ref”，意味着性能要好很多，因为ref代表着匹配少量的行。

至此，我们已经确认了MySQL选择了错误的索引，我们可以删除idx_register_date索引，或者新增一个idx_userid_registerDate索引，使SQL一定能走到USER_ID开头的索引中。
# 深层分析
为什么旧版页面快，新版页面在特定的查询条件下变慢了呢？<br />MySQL在执行语句之前，并不能准确地知道满足条件的记录有多少条，只能通过统计信息来估算记录数，在本案例的场景下，MySQL可能认为idx_register_date更有区分度，且预扫描的行数确实也更少，导致了其选择了错误的索引。<br />如果一个查询语句使用了ORDER BY和LIMIT子句，当优化器发现使用一个索引会加速查询执行的时候，会尝试使用它。<br />MySQL8.0以后可以通过以下命令关闭这个特性 SET optimizer_switch = "prefer_ordering_index=off"; 但是在8.0以前的版本，这个是写死的。
