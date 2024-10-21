# 👌mybatis的paramtertype为什么可以写简称？

# 口语化回答
好的，面试官。parameterType 用于指定传递给 SQL 语句的参数类型，但是经常传全类名非常的麻烦，mybatis 允许进行简化，以来的是类别名机制。myabtis 本身对于一些基础类型就默认了一些别名，比如 Integer，你写 int 也是没问题的。如果我们也想自己定义，就可以使用typeAlias 这个来进行类名和别名的映射。mybatis在解析属性时，会先检查是否存在别名。如果存在，则使用别名对应的类，如果不存在，则尝试直接使用属性值作为类名进行加载。以上。

# 面试得分点
别名机制，typealias

# 题目详细答案
在 MyBatis 配置文件中，parameterType 属性用于指定传递给 SQL 语句的参数类型。MyBatis 允许开发者使用全限定类名（例如 com.example.User）或简称（例如 User ）来指定参数类型。这种灵活性源于 MyBatis 的类型别名机制。

## 类型别名机制
MyBatis 提供了一种类型别名机制，允许开发者为常用的 Java 类型定义简短的别名，从而简化配置文件中的类型声明。这种机制不仅可以减少冗长的类名，还可以提高配置文件的可读性。

### 默认类型别名
MyBatis 内置了一些常用类型的默认别名，例如：

`java.lang.Integer`->`int`

`java.lang.String`->`string`

`java.util.Date`->`date`

`java.util.Map`->`map`

### 自定义类型别名
还可以在 MyBatis 配置文件中定义自定义类型别名。

1. **在 MyBatis 配置文件中定义类型别名**：

```plain
<typeAliases>
    <typeAlias alias="User" type="com.example.User"/>
    <typeAlias alias="Order" type="com.example.Order"/>
</typeAliases>
```

2. **在映射文件中使用类型别名**：

```plain
<select id="selectUserById" parameterType="User" resultType="User">
    SELECT * FROM users WHERE id = #{id}
</select>
```

在上述示例中，`User`被定义为`com.example.User`类的别名，因此在`parameterType`和`resultType`属性中可以直接使用`User`。

## 类型别名解析过程
MyBatis 在解析`parameterType`和`resultType`等属性时，会首先检查是否存在与之对应的类型别名。如果存在，则使用别名对应的类；如果不存在，则尝试直接使用属性值作为类名进行加载。

1. **初始化类型别名**： 在 MyBatis 配置文件中，`<typeAliases>`元素用于定义类型别名。MyBatis 会在启动时解析这些配置，并将别名与对应的类映射存储在一个内部映射表中。
2. **解析参数类型**： 当 MyBatis 解析`parameterType`等属性时，会首先检查属性值是否在别名映射表中。如果找到匹配的别名，则使用对应的类；如果未找到匹配的别名，则尝试将属性值作为类的全限定名进行加载。
3. **加载类**： 如果属性值不是别名，MyBatis 会尝试使用`Class.forName`方法加载类。如果加载成功，则使用该类；如果加载失败，则抛出异常。

### 代码 Demo
```plain
public class TypeAliasRegistry {
    private final Map<String, Class<?>> typeAliases = new HashMap<>();

    public void registerAlias(String alias, Class<?> type) {
        typeAliases.put(alias.toLowerCase(Locale.ENGLISH), type);
    }

    public Class<?> resolveAlias(String alias) {
        String key = alias.toLowerCase(Locale.ENGLISH);
        Class<?> type = typeAliases.get(key);
        if (type == null) {
            try {
                type = Class.forName(alias);
            } catch (ClassNotFoundException e) {
                throw new RuntimeException("Could not resolve type alias: " + alias, e);
            }
        }
        return type;
    }
}
```

在这个示例中，`TypeAliasRegistry`类维护了一个别名到类的映射表，并提供了`registerAlias`方法来注册别名。`resolveAlias`方法用于解析别名或直接加载类。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fqtro1gnbzf8w531>