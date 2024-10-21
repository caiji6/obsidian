# 👌SpringBoot中jar包的启动器子类

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring Boot 中，JAR 包的启动器子类主要用于加载和启动 Spring Boot 应用程序。Spring Boot 提供了多种启动器子类来支持不同类型的应用程序和打包方式。</font>

## <font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">JarLauncher</font>`
`<font style="background-color:rgb(247, 248, 249);">JarLauncher</font>`<font style="background-color:rgb(247, 248, 249);">是 Spring Boot 中默认的启动器，用于启动嵌入式 JAR 包中的应用程序。它通过加载和执行 JAR 包中的主类来启动应用程序。</font>

```plain
import org.springframework.boot.loader.JarLauncher;

public class CustomJarLauncher extends JarLauncher {
    public static void main(String[] args) {
        new CustomJarLauncher().launch(args);
    }
}
```

## <font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">WarLauncher</font>`
`<font style="background-color:rgb(247, 248, 249);">WarLauncher</font>`<font style="background-color:rgb(247, 248, 249);">用于启动嵌入式 WAR 包中的应用程序。它适用于那些打包为 WAR 格式并包含嵌入式服务器（如 Tomcat）的 Spring Boot 应用程序。</font>

```plain
import org.springframework.boot.loader.WarLauncher;

public class CustomWarLauncher extends WarLauncher {
    public static void main(String[] args) {
        new CustomWarLauncher().launch(args);
    }
}
```

## <font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">PropertiesLauncher</font>`
`<font style="background-color:rgb(247, 248, 249);">PropertiesLauncher</font>`<font style="background-color:rgb(247, 248, 249);">是一种通用的启动器，可以通过外部配置文件（如</font>`<font style="background-color:rgb(247, 248, 249);">application.properties</font>`<font style="background-color:rgb(247, 248, 249);">）来配置启动参数。它适用于需要高度可配置性的场景。</font>

```plain
import org.springframework.boot.loader.PropertiesLauncher;

public class CustomPropertiesLauncher extends PropertiesLauncher {
    public static void main(String[] args) {
        new CustomPropertiesLauncher().launch(args);
    }
}
```

## <font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">ExecutableArchiveLauncher</font>`
`<font style="background-color:rgb(247, 248, 249);">ExecutableArchiveLauncher</font>`<font style="background-color:rgb(247, 248, 249);">是一个抽象类，是所有其他启动器的基类。它提供了通用的启动逻辑，具体的启动器（如</font>`<font style="background-color:rgb(247, 248, 249);">JarLauncher</font>`<font style="background-color:rgb(247, 248, 249);">和</font>`<font style="background-color:rgb(247, 248, 249);">WarLauncher</font>`<font style="background-color:rgb(247, 248, 249);">）继承并实现了它。</font>

## <font style="background-color:rgb(247, 248, 249);">自定义启动器</font>
<font style="background-color:rgb(247, 248, 249);">还可以创建自己的启动器子类，以满足特定的需求。例如，可以自定义启动器来添加一些启动前或启动后的逻辑。</font>

```plain
import org.springframework.boot.loader.JarLauncher;

public class MyCustomLauncher extends JarLauncher {
    @Override
    protected void launch(String[] args) {
        // 在启动之前执行一些自定义逻辑
        System.out.println("Starting MyCustomLauncher...");

        // 调用父类的启动方法
        super.launch(args);

        // 在启动之后执行一些自定义逻辑
        System.out.println("MyCustomLauncher started.");
    }

    public static void main(String[] args) {
        new MyCustomLauncher().launch(args);
    }
}
```

## <font style="background-color:rgb(247, 248, 249);">启动器子类的应用场景</font>
**<font style="background-color:rgb(247, 248, 249);">JarLauncher</font>**<font style="background-color:rgb(247, 248, 249);">：用于启动大多数 Spring Boot 应用程序，特别是那些打包为 JAR 的应用程序。</font>

**<font style="background-color:rgb(247, 248, 249);">WarLauncher</font>**<font style="background-color:rgb(247, 248, 249);">：用于启动打包为 WAR 的 Spring Boot 应用程序，通常用于传统的企业应用程序。</font>

**<font style="background-color:rgb(247, 248, 249);">PropertiesLauncher</font>**<font style="background-color:rgb(247, 248, 249);">：用于需要通过外部配置文件进行高度定制的应用程序。</font>

**<font style="background-color:rgb(247, 248, 249);">自定义启动器</font>**<font style="background-color:rgb(247, 248, 249);">：用于需要在启动过程中执行特定逻辑的应用程序。</font>



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/rzfi5kto10eh13s8>