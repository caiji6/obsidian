# 👌Springboot使用java.ext.dirs方式的缺陷

# 题目详细答案
## <font style="background-color:rgb(247, 248, 249);">已被弃用和移除</font>
**<font style="background-color:rgb(247, 248, 249);">弃用和移除</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">java.ext.dirs</font>`<font style="background-color:rgb(247, 248, 249);">选项已经在 Java 9 中被弃用，并在后续版本中被移除。因此，依赖于</font>`<font style="background-color:rgb(247, 248, 249);">java.ext.dirs</font>`<font style="background-color:rgb(247, 248, 249);">的解决方案在现代 Java 版本中将无法工作。</font>

**<font style="background-color:rgb(247, 248, 249);">兼容性问题</font>**<font style="background-color:rgb(247, 248, 249);">：如果你的应用程序依赖于</font>`<font style="background-color:rgb(247, 248, 249);">java.ext.dirs</font>`<font style="background-color:rgb(247, 248, 249);">，那么在升级到更高版本的 Java 时可能会遇到兼容性问题。</font>

## <font style="background-color:rgb(247, 248, 249);">安全性问题</font>
**<font style="background-color:rgb(247, 248, 249);">全局类加载器</font>**<font style="background-color:rgb(247, 248, 249);">：使用</font>`<font style="background-color:rgb(247, 248, 249);">java.ext.dirs</font>`<font style="background-color:rgb(247, 248, 249);">方式会将 JAR 包添加到扩展类加载器中，这意味着这些 JAR 包将对所有应用程序可见。这会导致潜在的安全风险，因为不受信任的代码可能会被加载并执行。</font>

**<font style="background-color:rgb(247, 248, 249);">类冲突</font>**<font style="background-color:rgb(247, 248, 249);">：在扩展目录中添加 JAR 包可能会与其他应用程序使用的 JAR 包发生冲突，从而导致类加载问题和难以调试的错误。</font>

## <font style="background-color:rgb(247, 248, 249);">难以管理和维护</font>
**<font style="background-color:rgb(247, 248, 249);">全局配置</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">java.ext.dirs</font>`<font style="background-color:rgb(247, 248, 249);">是一个全局配置，影响所有运行在同一 JVM 上的应用程序。这使得管理和维护变得复杂，因为你需要确保所有应用程序都兼容这些扩展 JAR 包。</font>

**<font style="background-color:rgb(247, 248, 249);">不可预测的行为</font>**<font style="background-color:rgb(247, 248, 249);">：由于扩展目录中的 JAR 包对所有应用程序可见，可能会导致不可预测的行为，特别是在不同应用程序之间存在依赖冲突的情况下。</font>

## <font style="background-color:rgb(247, 248, 249);">使用 Spring Boot 的 ClassLoader</font>
<font style="background-color:rgb(247, 248, 249);">如果需要加载外部 JAR 包，可以在 Spring Boot 应用程序中使用自定义的类加载器。使用</font>`<font style="background-color:rgb(247, 248, 249);">URLClassLoader</font>`<font style="background-color:rgb(247, 248, 249);">来加载外部 JAR 包：</font>

```plain
import java.net.URL;
import java.net.URLClassLoader;

public class CustomClassLoader {
    public static void main(String[] args) throws Exception {
        URL[] urls = {new URL("file:///path/to/external.jar")};
        URLClassLoader urlClassLoader = new URLClassLoader(urls, Thread.currentThread().getContextClassLoader());
        Thread.currentThread().setContextClassLoader(urlClassLoader);

        // 启动 Spring Boot 应用程序
        SpringApplication.run(MyApplication.class, args);
    }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/lrp1ld8xr67dbb1v>