MyBatis 的二级缓存是一个跨会话（Session）级别的缓存机制，用于减少数据库访问次数，提高应用程序性能。它与一级缓存的主要区别在于其作用范围和持久性。
### 一级缓存 vs 二级缓存
#### 一级缓存（Local Cache）

- **作用范围**：一级缓存是会话级别的缓存，作用范围仅限于同一个SqlSession。
- **生命周期**：一级缓存的生命周期与SqlSession一致。当SqlSession关闭时，一级缓存也会被清空。
- **默认开启**：一级缓存默认是开启的，无需额外配置。
#### 二级缓存（Global Cache）

- **作用范围**：二级缓存是跨会话级别的缓存，作用范围是整个SqlSessionFactory。不同的SqlSession实例可以共享二级缓存。
- **生命周期**：二级缓存的生命周期与SqlSessionFactory一致，通常在应用程序运行期间一直存在。
- **需显式配置**：二级缓存需要在 MyBatis 配置文件中显式开启，并且可以配置不同的缓存提供者（如 Ehcache、Hazelcast 等）。
### 二级缓存的配置
要使用 MyBatis 的二级缓存，需要进行以下配置：

1. **在 MyBatis 配置文件中启用二级缓存**：
```
<settings>
    <setting name="cacheEnabled" value="true"/>
</settings>
```

1. **在 Mapper 文件中配置二级缓存**：

在每个需要使用二级缓存的 Mapper 文件中添加<cache>标签：
```
<mapper namespace="com.example.mapper.UserMapper">
    <cache/>
    
    <select id="selectUserById" parameterType="int" resultType="com.example.model.User">
        SELECT id, username, email FROM users WHERE id = #{id}
    </select>
</mapper>
```
### 二级缓存的工作原理

- **存储机制**：二级缓存会将查询结果存储在内存中，以键值对（Key-Value）的形式保存。Key 通常是查询语句及其参数的组合，Value 是查询结果。
- **缓存失效**：当执行更新、插入或删除操作时，相关的缓存条目会被自动清除，以确保数据一致性。
- **缓存提供者**：MyBatis 提供了默认的缓存实现，但也可以集成第三方缓存框架，如 Ehcache、Hazelcast 等，通过配置<cache>标签中的type属性来指定缓存提供者。
### 示例
假设有一个UserMapper接口和对应的 XML 配置文件：
```
public interface UserMapper {
    User selectUserById(int id);
}
```
UserMapper.xml配置文件：
```
<mapper namespace="com.example.mapper.UserMapper">
    <cache/>
    
    <select id="selectUserById" parameterType="int" resultType="com.example.model.User">
        SELECT id, username, email FROM users WHERE id = #{id}
    </select>
</mapper>
```
在这个示例中，selectUserById查询的结果会被缓存到二级缓存中。下次相同的查询请求将直接从缓存中获取结果，而不需要再次访问数据库。
### 配置缓存提供者（以 Ehcache 为例）
首先，添加 Ehcache 依赖：
```
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>
```
然后，在UserMapper.xml中配置 Ehcache 作为缓存提供者：
```
<mapper namespace="com.example.mapper.UserMapper">
    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
    
    <select id="selectUserById" parameterType="int" resultType="com.example.model.User">
        SELECT id, username, email FROM users WHERE id = #{id}
    </select>
</mapper>
```
此外，还需要配置 Ehcache 的ehcache.xml文件：
```
<ehcache>
    <cache name="com.example.mapper.UserMapper"
           maxEntriesLocalHeap="1000"
           timeToLiveSeconds="300">
    </cache>
</ehcache>
```
