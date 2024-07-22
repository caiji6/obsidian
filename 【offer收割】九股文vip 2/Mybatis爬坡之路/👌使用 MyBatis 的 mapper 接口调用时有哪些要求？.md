### 1. Mapper 接口要求
#### 接口定义

- **接口必须是公共的**：Mapper 接口必须用public关键字修饰。
- **方法签名**：接口中的方法签名必须与映射文件中的 SQL 语句 ID 一致。
```
package com.example.mapper;

import com.example.model.User;
import org.apache.ibatis.annotations.Select;

public interface UserMapper {
    User selectUser(int id);
}
```
### 2. 映射文件要求
#### 命名空间

- **命名空间**：映射文件的命名空间必须与 Mapper 接口的全限定名一致。
```
<mapper namespace="com.example.mapper.UserMapper">
  <select id="selectUser" parameterType="int" resultType="com.example.model.User">
    SELECT * FROM users WHERE id = #{id}
  </select>
</mapper>
```
#### SQL 语句 ID

- **SQL 语句 ID**：映射文件中的 SQL 语句 ID 必须与 Mapper 接口中的方法名一致。
### 3. 配置文件要求
#### 注册 Mapper

- **注册 Mapper**：在 MyBatis 配置文件中，必须注册 Mapper 接口或映射文件。
```
<mappers>
  <mapper resource="com/example/mapper/UserMapper.xml"/>
</mappers>
```
或者可以使用包扫描的方式：
```
<mappers>
  <package name="com.example.mapper"/>
</mappers>
```
### 4. 参数和返回类型
#### 参数类型

- **参数类型**：Mapper 接口方法的参数类型必须与映射文件中的parameterType一致。如果使用注解方式，参数名要与 SQL 中的占位符一致。
#### 返回类型

- **返回类型**：Mapper 接口方法的返回类型必须与映射文件中的resultType一致。
