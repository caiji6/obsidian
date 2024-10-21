# 👌如何在 Spring MVC 中实现国际化（i18n）？

# 题目详细答案
1. **<font style="background-color:rgb(247, 248, 249);">创建资源文件</font>**<font style="background-color:rgb(247, 248, 249);">：首先，你需要创建多个资源文件来存储不同语言的文本信息。这些文件通常以 </font>`<font style="background-color:rgb(247, 248, 249);">.properties</font>`<font style="background-color:rgb(247, 248, 249);"> 扩展名结尾，并且每个文件都对应一个特定的语言和国家代码（如 </font>`<font style="background-color:rgb(247, 248, 249);">messages_en_US.properties</font>`<font style="background-color:rgb(247, 248, 249);">）。在这些文件中，你可以定义 key-value 对，其中 key 是一个唯一的标识符，value 是相应语言的翻译文本。</font>
2. **<font style="background-color:rgb(247, 248, 249);">配置消息源</font>**<font style="background-color:rgb(247, 248, 249);">：在 Spring MVC 中，你需要配置一个 </font>`<font style="background-color:rgb(247, 248, 249);">MessageSource</font>`<font style="background-color:rgb(247, 248, 249);"> bean 来加载这些资源文件。最常用的实现是 </font>`<font style="background-color:rgb(247, 248, 249);">ReloadableResourceBundleMessageSource</font>`<font style="background-color:rgb(247, 248, 249);">，它允许你在不重新启动应用程序的情况下更新资源文件。</font>

```plain
<bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
    <property name="basenames" value="com.example.i18n.messages"/>
    <property name="defaultEncoding" value="UTF-8"/>
    <property name="fallbackToSystemLocale" value="true"/>
</bean>
```

<font style="background-color:rgb(247, 248, 249);"></font>`<font style="background-color:rgb(247, 248, 249);">basenames</font>`<font style="background-color:rgb(247, 248, 249);"> 属性指向了你的资源文件的基本名称（不包括语言和国家代码部分），</font>`<font style="background-color:rgb(247, 248, 249);">defaultEncoding</font>`<font style="background-color:rgb(247, 248, 249);"> 属性指定了文件的编码方式，</font>`<font style="background-color:rgb(247, 248, 249);">fallbackToSystemLocale</font>`<font style="background-color:rgb(247, 248, 249);"> 属性决定了当请求的语言没有对应的资源文件时是否使用系统默认的语言。</font>



3. **<font style="background-color:rgb(247, 248, 249);">使用 </font>**`**<font style="background-color:rgb(247, 248, 249);">@SessionAttribute</font>**`**<font style="background-color:rgb(247, 248, 249);"> 或 </font>**`**<font style="background-color:rgb(247, 248, 249);">@RequestAttribute</font>**`**<font style="background-color:rgb(247, 248, 249);"> 注解获取当前语言</font>**<font style="background-color:rgb(247, 248, 249);">：为了在处理器方法中获取当前的语言环境，你可以使用 </font>`<font style="background-color:rgb(247, 248, 249);">@SessionAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 或 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestAttribute</font>`<font style="background-color:rgb(247, 248, 249);"> 注解来注入当前的 </font>`<font style="background-color:rgb(247, 248, 249);">Locale</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>

```plain
@Controller
public class MyController {
    @GetMapping("/greeting")
    public String greeting(@RequestAttribute("locale") Locale locale) {
        // 使用 locale 对象来获取相应语言的文本信息
    }
}
```



4. **<font style="background-color:rgb(247, 248, 249);">在视图中使用国际化文本</font>**<font style="background-color:rgb(247, 248, 249);">：在 JSP、Thymeleaf 或其他视图技术中，你可以使用 Spring 提供的国际化标签或函数来显示相应语言的文本信息。例如，在 JSP 中，你可以使用 </font>`<font style="background-color:rgb(247, 248, 249);"><spring:message></font>`<font style="background-color:rgb(247, 248, 249);"> 标签：</font>

```plain
<spring:message code="greeting.message" text="Hello, World!" />
```

`<font style="background-color:rgb(247, 248, 249);">code</font>`<font style="background-color:rgb(247, 248, 249);"> 属性指定了要查找的文本的 key，</font>`<font style="background-color:rgb(247, 248, 249);">text</font>`<font style="background-color:rgb(247, 248, 249);"> 属性提供了一个默认值，以防没有找到相应的翻译文本。</font>

<font style="background-color:rgb(247, 248, 249);"></font>

5. **<font style="background-color:rgb(247, 248, 249);">处理请求的语言</font>**<font style="background-color:rgb(247, 248, 249);">：最后，你需要在应用程序中处理请求的语言。通常情况下，这可以通过在请求头中设置</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">Accept-Language</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">来实现。Spring MVC 提供了一个</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">LocaleChangeInterceptor</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">来自动检测和处理这种情况。你只需要将其添加到你的拦截器链中即可：</font>

```plain
<mvc:interceptors>
    <bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor"/>
</mvc:interceptors>
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/li46tk6o1v735brl>