# 👌SpringBoot如何固定版本

# 题目详细答案
在 Spring Boot 项目中，固定版本主要是为了确保项目依赖的库版本一致，避免因版本不一致导致的兼容性问题。

## 使用`spring-boot-starter-parent`
使用`spring-boot-starter-parent`是最常见的方法之一。它不仅提供了一组默认的依赖版本，还包括了一些有用的插件配置。你可以在`pom.xml`中指定 Spring Boot 的版本：

```plain
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.5</version> <!-- 这里指定了Spring Boot的版本 -->
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

这样，所有 Spring Boot 相关的依赖都会使用这个版本中定义的版本号。

## 使用`dependencyManagement`
如果你不想使用`spring-boot-starter-parent`作为父 POM，或者你的项目已经有了其他的父 POM，你可以使用`dependencyManagement`来管理依赖版本。这样可以手动指定各个依赖的版本：

```plain
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.7.5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

然后在你的`dependencies`部分添加具体的依赖时，不需要再指定版本号：

```plain
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- 其他依赖 -->
</dependencies>
```

## 手动指定依赖版本
如果你希望完全控制所有依赖的版本，可以手动在`dependencies`部分指定每个依赖的版本号：

```plain
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.7.5</version>
    </dependency>
    <!-- 其他依赖 -->
</dependencies>
```

这种方法虽然灵活，但需要手动管理每个依赖的版本，比较繁琐，且容易出错。

## 使用 BOM
Spring Boot 提供了一个 BOM（Bill of Materials），可以用来统一管理依赖的版本。你可以在`dependencyManagement`中引入 Spring Boot 的 BOM：

```plain
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.7.5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

然后在`dependencies`部分添加具体的依赖时，不需要再指定版本号：

```plain
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- 其他依赖 -->
</dependencies>
```

最推荐的方法是使用`spring-boot-starter-parent`或者`dependencyManagement`来管理依赖版本，这样可以减少手动管理版本的工作量，并且更容易保持依赖的一致性。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/tpi4mip0hmx5vcb4>