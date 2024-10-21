# 👌Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？

# 口语化答案
好的，面试官。可以重复，每个 XML 映射文件有一个唯一的命名空间，确保不同文件中的id不会冲突。这个就有点像 java 的包的概念，回想一下，包名不同，里面的类相同也没事。同时每个 Mapper 接口对应一个 XML 映射文件，接口的方法名与 XML 文件中的id相对应。这样就已经隔离开了，即使 id 重复也不会有冲突。以上。

# 题目解析
算是一道考察你有没有思考过这种分离概念，很多小伙伴确实是会不断的配置，但是对于这种内容的思考就晚起没想过。

# 面试得分点
命名空间概念

# 题目详细答案
在 MyBatis 的 XML 映射文件中，不同的 XML 映射文件中的id是可以重复的。每个 XML 映射文件都有一个独立的命名空间（namespace），这个命名空间通常对应于 Mapper 接口的全限定名。命名空间确保了不同映射文件中的id不会冲突。

**命名空间（namespace）**：每个 XML 映射文件都有一个唯一的命名空间，通常是对应的 Mapper 接口的全限定名。命名空间的存在使得即使在不同的 XML 映射文件中有相同的id，也不会引起冲突。

例如：

```plain
<!-- UserMapper.xml -->
<mapper namespace="com.example.mapper.UserMapper">
  <select id="selectById" parameterType="int" resultType="com.example.model.User">
    SELECT * FROM users WHERE user_id = #{id}
  </select>
</mapper>
```

```plain
<!-- OrderMapper.xml -->
<mapper namespace="com.example.mapper.OrderMapper">
  <select id="selectById" parameterType="int" resultType="com.example.model.Order">
    SELECT * FROM orders WHERE order_id = #{id}
  </select>
</mapper>
```

UserMapper和OrderMapper都有一个id为selectById的<select>语句，但因为它们位于不同的命名空间下，所以不会冲突。



**Mapper 接口**：每个 Mapper 接口对应一个 XML 映射文件，接口的方法名与 XML 文件中的id相对应。

```plain
// UserMapper.java
public interface UserMapper {
    User selectById(int id);
}
```

```plain
// OrderMapper.java
public interface OrderMapper {
    Order selectById(int id);
}
```

这样做可以确保即使在不同的 XML 文件中有相同的id，也不会引起冲突，因为它们是由不同的 Mapper 接口调用的。

### 


> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/cckuy4d5zfuy5s2g>