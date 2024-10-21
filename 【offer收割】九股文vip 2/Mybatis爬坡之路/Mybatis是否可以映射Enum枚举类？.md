# 👌Mybatis是否可以映射Enum枚举类？

# 口语化回答
好的，面试官，mybatis 是可以映射枚举的。如果数据库字段是字符串类型，直接使用 enumtypehandler 指定好枚举之后，就可以自动映射，如果美剧是包含属性这种情况，我们需要自己实现一个TypeHandler 来继承 BaseTypeHandler，实现其中的如何从枚举映射的规则。在后面的版本中，mybatis 给我们提供了两个新东西。

EnumOrdinalTypeHandler和EnumTypeHandler。EnumOrdinalTypeHandler 可以将枚举的序数（ordinal）存储到数据库中。EnumTypeHandler 会将枚举的名称（name）存储到数据库中。以上。

# 题目解析
低频题，问的也不多，不过实际中倒是比较常用，通过枚举来进行统一管理，有些小伙伴会在 sql 里面写死=几，这种就不行。

# 面试得分点
TypeHandler，映射

# 题目详细答案
可以映射。

## 使用内置的EnumTypeHandler
### 映射到字符串
假设有一个UserStatus枚举类：

```plain
public enum UserStatus {
    ACTIVE,
    INACTIVE,
    DELETED
}
```

在数据库中，status字段是字符串类型。可以在 MyBatis 的 XML 配置文件中指定使用EnumTypeHandler：

```plain
<resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="status" column="status" typeHandler="org.apache.ibatis.type.EnumTypeHandler"/>
</resultMap>
```

或者在注解中使用：

```plain
@Results({
    @Result(property = "id", column = "id"),
    @Result(property = "status", column = "status", typeHandler = EnumTypeHandler.class)
})
@Select("SELECT id, status FROM users WHERE id = #{id}")
User selectUserById(int id);
```

### 映射到整数
假设UserStatus枚举类有一个整数值：

```plain
public enum UserStatus {
    ACTIVE(1),
    INACTIVE(2),
    DELETED(3);

    private final int value;

    UserStatus(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

可以创建一个自定义的TypeHandler来处理枚举和整数之间的转换：

```plain
public class UserStatusTypeHandler extends BaseTypeHandler<UserStatus> {
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, UserStatus parameter, JdbcType jdbcType) throws SQLException {
        ps.setInt(i, parameter.getValue());
    }

    @Override
    public UserStatus getNullableResult(ResultSet rs, String columnName) throws SQLException {
        int value = rs.getInt(columnName);
        return UserStatus.fromValue(value);
    }

    @Override
    public UserStatus getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        int value = rs.getInt(columnIndex);
        return UserStatus.fromValue(value);
    }

    @Override
    public UserStatus getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        int value = cs.getInt(columnIndex);
        return UserStatus.fromValue(value);
    }
}
```

在枚举类中添加一个静态方法来根据整数值获取枚举实例：

```plain
public static UserStatus fromValue(int value) {
    for (UserStatus status : UserStatus.values()) {
        if (status.getValue() == value) {
            return status;
        }
    }
    throw new IllegalArgumentException("Unknown enum value: " + value);
}
```

然后在 MyBatis 配置文件中使用自定义的TypeHandler：

```plain
<resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="status" column="status" typeHandler="com.example.typehandler.UserStatusTypeHandler"/>
</resultMap>
```

## EnumOrdinalTypeHandler和EnumTypeHandler
MyBatis 3.4.5 及以上版本提供了两个新的TypeHandler：EnumOrdinalTypeHandler和EnumTypeHandler。

**EnumOrdinalTypeHandler**：将枚举的序数（ordinal）存储到数据库中。

**EnumTypeHandler**：将枚举的名称（name）存储到数据库中。

可以在配置文件中指定使用这些TypeHandler：

```plain
<resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="status" column="status" typeHandler="org.apache.ibatis.type.EnumOrdinalTypeHandler"/>
</resultMap>
```

## 全局配置枚举处理器
可以在 MyBatis 全局配置文件中指定默认的枚举处理器，这样就不需要在每个resultMap或注解中重复指定TypeHandler。

```plain
<typeHandlers>
    <typeHandler javaType="com.example.model.UserStatus" handler="org.apache.ibatis.type.EnumTypeHandler"/>
</typeHandlers>
```





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gichgitnzgkwwhrf>