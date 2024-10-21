# 👌mybatis与jdbc编程相比有什么优势？

# 口语化回答
好的，面试官，相比于传统的 jdbc，mybatis 大大提升了开发效率，省去了传统开发繁琐的操作。同时 mybatis 的使用方式会将 sql 和代码分离出来，这种情况极大的提高了可维护性。还提供了一级，二级缓存机制，提升了性能。除此之外还有属性的自动映射封装，提供一堆增强插件等。以上。

# 题目解析
高频题，选下面的优点，答出 2，3 个即可。

# 面试得分点
简化代码，sql 与代码分离，自动映射，动态 sql 支持，缓存支持，插件支持

# 题目详细答案
MyBatis 是一个流行的持久层框架，它在简化数据库操作、提高开发效率和维护性方面具有显著优势。与传统的 JDBC 编程相比，MyBatis 提供了许多改进和便利。

## 简化代码
在 JDBC 编程中，开发者需要编写大量的样板代码来处理数据库连接、语句创建、参数设置、结果集处理和资源关闭等操作。MyBatis 通过简化这些操作，减少了样板代码的数量，使代码更加简洁和易于维护。

**JDBC 示例：**

```plain
Connection conn = null;
PreparedStatement pstmt = null;
ResultSet rs = null;
try {
    conn = DriverManager.getConnection(DB_URL, USER, PASS);
    String sql = "SELECT id, name, age FROM users WHERE id = ?";
    pstmt = conn.prepareStatement(sql);
    pstmt.setInt(1, userId);
    rs = pstmt.executeQuery();
    if (rs.next()) {
        int id = rs.getInt("id");
        String name = rs.getString("name");
        int age = rs.getInt("age");
        // Process the result
    }
} catch (SQLException e) {
    e.printStackTrace();
} finally {
    try {
        if (rs != null) rs.close();
        if (pstmt != null) pstmt.close();
        if (conn != null) conn.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```

**MyBatis 示例：**

```plain
SqlSession session = sqlSessionFactory.openSession();
try {
    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selectUserById(userId);
    // Process the result
} finally {
    session.close();
}
```

## SQL 与代码的分离
MyBatis 允许将 SQL 语句放在 XML 配置文件或注解中，从而将 SQL 与 Java 代码分离。这种分离使得 SQL 更加清晰易读，也更容易进行修改和维护。

## 自动映射
MyBatis 提供了强大的结果映射功能，可以将查询结果自动映射到 Java 对象中。不需要手动处理结果集和对象之间的转换。

**JDBC 手动映射：**

```plain
User user = new User();
user.setId(rs.getInt("id"));
user.setName(rs.getString("name"));
user.setAge(rs.getInt("age"));
```

**MyBatis 自动映射：**

```plain
// 在 XML 配置文件中配置结果映射
<resultMap id="UserResultMap" type="User">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="age" column="age"/>
</resultMap>

// 使用结果映射
<select id="selectUserById" resultMap="UserResultMap">
    SELECT id, name, age FROM users WHERE id = #{id}
</select>
```

## 动态 SQL 支持
MyBatis 提供了强大的动态 SQL 支持，可以根据条件生成不同的 SQL 语句。这使得构建复杂查询变得更加容易和灵活。

```plain
<select id="findUsers" parameterType="map" resultType="User">
    SELECT * FROM users
    <where>
        <if test="name != null">
            AND name = #{name}
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </where>
</select>
```

## 缓存支持
MyBatis 提供了一级缓存和二级缓存机制，可以显著提高数据库查询的性能。一级缓存是 SqlSession 级别的缓存，二级缓存是全局级别的缓存。

## 插件机制
MyBatis 提供了插件机制，允许开发者自定义行为，例如日志记录、性能监控、参数检查等。通过插件，开发者可以在 SQL 执行的各个阶段插入自定义逻辑。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hyuipol5qdqype1t>