# 👌SpringBoot外部配置文件加载顺序

# 题目详细答案
SpringBoot外部配置文件的加载顺序，从高到低大致如下

### 命令行参数：
通过命令行传入的参数具有最高的优先级，可以覆盖其他配置文件中相同的配置项。例如，使用`java -jar myapp.jar --server.port=8080`来设置服务端口。

### Java系统属性（`System.getProperties()`）：
可以通过`System.setProperty`方法设置，或者在启动JVM时通过`-D`参数指定，例如`-Duser.name=admin`。

### 操作系统环境变量：
系统环境变量中的配置，优先级低于Java系统属性。

### 外部配置文件（jar包外部）：
包括`application-{profile}.properties`或`application-{profile}.yml`（带有profile的）和`application.properties`或`application.yml`（不带profile的），这些文件从jar包外向jar包内寻找，带profile的配置文件优先于不带profile的。

### 外部配置文件（jar包内部）：
与jar包外部的配置文件类似，但优先级更低。

### 通过`@Configuration`注解类上的`@PropertySource`指定的配置文件：
这些配置文件的优先级低于上述所有外部配置方式。

### 通过`SpringApplication.setDefaultProperties`指定的默认属性：
这是Spring Boot提供的另一种设置默认属性的方式，优先级最低。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fyac4e7exfhv1gpc>