### 1.使用XML配置文件中的<context:property-placeholder>标签：
在Spring的XML配置文件中，可以通过<context:property-placeholder>标签来引入外部配置文件。这种方式通常用于传统Spring项目中，它允许开发者在XML配置文件中指定外部属性文件的路径，并在其他配置中通过${propertyKey}的形式引用这些属性。
```xml
<context:property-placeholder location="classpath:config.properties"/>
```
这会将classpath下的config.properties文件加载到Spring的上下文中，文件中的属性就可以在其他配置中通过占位符引用了。
### 2.使用@PropertySource注解：
在基于Java配置的Spring项目中，@PropertySource注解提供了一种声明式的方式来引入外部配置文件。可以在配置类上使用@PropertySource注解，并指定外部配置文件的路径。
```java
@Configuration  
@PropertySource("classpath:config.properties")  
public class AppConfig {  
    // 配置类内容  
}
```
这种方式适用于Spring 3.1及以上版本，它使得配置更加灵活和模块化。
### 3.使用Spring Boot的application.properties或application.yml文件：
Spring Boot项目默认会在src/main/resources目录下查找application.properties或application.yml文件作为外部配置文件。这些文件包含了项目的配置信息，Spring Boot会自动加载这些文件中的配置。<br />还可以在启动时通过命令行参数、环境变量或配置文件外部化配置（如--spring.config.location）来指定外部配置文件的路径。
### 4.使用Spring Cloud Config：
对于分布式系统，Spring Cloud Config提供了一种集中管理配置的方式。它允许开发者将配置信息存储在Git、SVN等版本控制系统中，并通过一个配置服务器来提供配置信息给客户端。<br />客户端在启动时会从配置服务器拉取配置信息，并根据需要更新自己的配置。这种方式非常适合于微服务架构下的配置管理。
### 5.通过环境变量：
通过环境变量来传递配置信息。Spring Boot支持通过环境变量来覆盖application.properties或application.yml文件中的配置。<br />环境变量的命名需要遵循一定的规则（如将.替换为_，并大写字母表示），以便Spring Boot能够正确解析。

