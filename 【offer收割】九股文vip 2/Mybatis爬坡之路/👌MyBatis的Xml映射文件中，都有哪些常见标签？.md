### 1.<mapper>
- **作用**：定义一个映射文件的根元素，包含所有的映射配置。
- **属性**：namespace，指定与之关联的 Mapper 接口的全限定名。
```
<mapper namespace="com.example.mapper.UserMapper">
  <!-- SQL 语句和其他配置 -->
</mapper>
```
### 2.<select>

- **作用**：定义一个查询语句。
- **常用属性**：
   - id：唯一标识该 SQL 语句的 ID。
   - parameterType：指定输入参数的类型（可选）。
   - resultType：指定返回结果的类型（常用）。
   - resultMap：指定自定义的结果映射（可选）。
```
<select id="selectUser" parameterType="int" resultType="com.example.model.User">
  SELECT * FROM users WHERE id = #{id}
</select>
```
### 3.<insert>

- **作用**：定义一个插入语句。
- **常用属性**：
   - id：唯一标识该 SQL 语句的 ID。
   - parameterType：指定输入参数的类型（常用）。
   - useGeneratedKeys和keyProperty：用于获取自动生成的主键值。
```
<insert id="insertUser" parameterType="com.example.model.User" useGeneratedKeys="true" keyProperty="id">
  INSERT INTO users (username, password) VALUES (#{username}, #{password})
</insert>
```
### 4.<update>

- **作用**：定义一个更新语句。
- **常用属性**：
   - id：唯一标识该 SQL 语句的 ID。
   - parameterType：指定输入参数的类型（常用）。
```
<update id="updateUser" parameterType="com.example.model.User">
  UPDATE users SET username = #{username}, password = #{password} WHERE id = #{id}
</update>
```
### 5.<delete>

- **作用**：定义一个删除语句。
- **常用属性**：
   - id：唯一标识该 SQL 语句的 ID。
   - parameterType：指定输入参数的类型（常用）。
```
<delete id="deleteUser" parameterType="int">
  DELETE FROM users WHERE id = #{id}
</delete>
```
### 6.<resultMap>

- **作用**：定义复杂的结果集映射，适用于复杂的对象结构。
- **常用子标签**：
   - <id>：标识主键字段。
   - <result>：标识普通字段。
   - <association>：标识对象关联。
   - <collection>：标识集合关联。
```
<resultMap id="userResultMap" type="com.example.model.User">
  <id property="id" column="id"/>
  <result property="username" column="username"/>
  <result property="password" column="password"/>
</resultMap>
```
### 7. 动态 SQL 标签
#### <if>

- **作用**：根据条件动态包含 SQL 片段。
- **属性**：test，指定条件表达式。
```
<select id="findUsers" resultType="com.example.model.User">
  SELECT * FROM users
  WHERE 1=1
  <if test="username != null">
    AND username = #{username}
  </if>
  <if test="password != null">
    AND password = #{password}
  </if>
</select>
```
#### <choose>,<when>,<otherwise>

- **作用**：类似于 switch-case 语句，根据条件选择执行不同的 SQL 片段。
- **属性**：test，指定条件表达式（用于<when>标签）。
```
<select id="findUsers" resultType="com.example.model.User">
  SELECT * FROM users
  WHERE 1=1
  <choose>
    <when test="username != null">
      AND username = #{username}
    </when>
    <when test="password != null">
      AND password = #{password}
    </when>
    <otherwise>
      AND status = 'active'
    </otherwise>
  </choose>
</select>
```
#### <trim>,<where>,<set>

- **作用**：用于生成动态 SQL 片段，自动处理 SQL 语句中的多余字符（如逗号、AND、OR 等）。
- **属性**：
   - <trim>：prefix、suffix、prefixOverrides、suffixOverrides。
   - <where>：自动添加 WHERE 关键字。
   - <set>：自动添加 SET 关键字并处理逗号。
```
<!-- 使用 <where> -->
<select id="findUsers" resultType="com.example.model.User">
  SELECT * FROM users
  <where>
    <if test="username != null">
      username = #{username}
    </if>
    <if test="password != null">
      AND password = #{password}
    </if>
  </where>
</select>

<!-- 使用 <set> -->
<update id="updateUser" parameterType="com.example.model.User">
  UPDATE users
  <set>
    <if test="username != null">
      username = #{username},
    </if>
    <if test="password != null">
      password = #{password}
    </if>
  </set>
  WHERE id = #{id}
</update>
```
### 8.<foreach>

- **作用**：用于遍历集合生成动态 SQL 语句，常用于 IN 子句等。
- **属性**：item、index、collection、open、close、separator。
```
<select id="findUsersByIds" resultType="com.example.model.User">
  SELECT * FROM users WHERE id IN
  <foreach item="id" collection="list" open="(" separator="," close=")">
    #{id}
  </foreach>
</select>
```
### 9.<sql>

- **作用**：定义可重用的 SQL 片段。
- **属性**：id，唯一标识该 SQL 片段。
```
<sql id="userColumns">
  id, username, password
</sql>

<select id="selectUser" resultType="com.example.model.User">
  SELECT <include refid="userColumns"/> FROM users WHERE id = #{id}
</select>
```
### 10.<include>

- **作用**：引入<sql>标签定义的 SQL 片段。
- **属性**：refid，引用<sql>标签的 ID。
```
<select id="selectUser" resultType="com.example.model.User">
  SELECT <include refid="userColumns"/> FROM users WHERE id = #{id}
</select>
```
