# 👌Mybatis是否支持延迟加载？实现原理是什么？

# 口语化答案
好的，面试官。mybatis 是支持延迟加载的。延迟加载的意义是在真正使用数据的时候，才进行加载，在 mybatis 场景中主要是关联的对象和集合。从技术方面，主要是通过代理来进行实现的。mybatis 关键的两个属性，一个是lazyLoadingEnabled，控制是否开启延迟加载，一个是 aggressiveLazyLoading 控制是否所有属性都走延迟加载。比如我们在查询的时候一个对象，关联了另一个对象，当我们在去操作属性的时候，如果未操作关联对象属性，就不会产生查询，如果操作了才会触发查询，以上。

# 题目解析
中频题，对于对象和集合的情况非常的常用，不过现在实际工作情况，基本上是用的比较少的，从维护和理解角度都很麻烦。

# 面试得分点
延迟加载意义，mybtis 的属性配置，demo 的展示

# 题目详细答案
MyBatis 支持延迟加载（Lazy Loading）。延迟加载是一种优化技术，只有在真正需要使用数据的时候才进行加载，从而提高系统性能和响应速度，MyBatis 的延迟加载主要用于关联对象和集合。

## 延迟加载的实现原理
MyBatis 的延迟加载主要通过动态代理和 CGLIB 代理来实现。

**动态代理**：MyBatis 使用 Java 的动态代理技术来创建对象的代理。当调用代理对象的方法时，代理对象会拦截调用，并在需要时触发真实对象的加载。

**CGLIB 代理**：对于没有接口的类，MyBatis 使用 CGLIB 生成子类代理。代理类会在调用方法时拦截调用，并在需要时触发真实对象的加载。

## 延迟加载的配置
延迟加载可以在 MyBatis 的全局配置文件（mybatis-config.xml）中进行配置：

```plain
<configuration>
    <settings>
        <!-- 启用延迟加载 -->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!-- 启用代理延迟加载 -->
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
</configuration>
```

lazyLoadingEnabled：是否启用延迟加载。默认值是false。

aggressiveLazyLoading：是否启用积极的延迟加载。默认值是false。如果设置为true，则只要调用任何延迟加载的属性，所有延迟加载的属性都会被加载。

## 延迟加载的使用
在 Mapper 文件中，可以通过fetchType属性来配置延迟加载，fetchType可以设置为lazy或eager。lazy表示延迟加载，eager表示立即加载。

```plain
<resultMap id="userResultMap" type="User">
    <id property="id" column="id"/>
    <result property="username" column="username"/>
    <result property="email" column="email"/>
    <association property="address" column="address_id" javaType="Address" fetchType="lazy">
        <id property="id" column="id"/>
        <result property="street" column="street"/>
        <result property="city" column="city"/>
    </association>
</resultMap>
```



## 延迟加载 Demo
### 配置文件（mybatis-config.xml）
```plain
<configuration>
    <settings>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
</configuration>
```

### Mapper 文件（UserMapper.xml）
```plain
<mapper namespace="com.example.UserMapper">
    <resultMap id="userResultMap" type="User">
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="email" column="email"/>
        <association property="address" column="address_id" javaType="Address" fetchType="lazy">
            <id property="id" column="id"/>
            <result property="street" column="street"/>
            <result property="city" column="city"/>
        </association>
    </resultMap>

    <select id="selectUserById" resultMap="userResultMap">
        SELECT * FROM users WHERE id = #{id}
    </select>
</mapper>
```

### Java 代码
```plain
try (SqlSession session = sqlSessionFactory.openSession()) {
    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selectUserById(1);

    // 此时 Address 还未加载
    System.out.println(user.getUsername());

    // 当访问 Address 属性时，触发延迟加载
    System.out.println(user.getAddress().getStreet());
}
```

### 


> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ssai1rb3acy27rw3>