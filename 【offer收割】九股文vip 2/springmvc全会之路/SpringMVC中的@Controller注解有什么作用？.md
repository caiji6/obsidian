# 👌Spring MVC 中的 @Controller 注解有什么作用？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> 注解用于标记一个类作为控制器组件。控制器是处理 HTTP 请求的核心组件，它负责接收请求、处理业务逻辑并返回视图或数据响应。</font>

## <font style="background-color:rgb(247, 248, 249);">主要作用</font>
1. **<font style="background-color:rgb(247, 248, 249);">标识控制器类</font>**<font style="background-color:rgb(247, 248, 249);">：</font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解告诉 Spring 该类是一个控制器，应该由 Spring 容器管理。</font>
2. **<font style="background-color:rgb(247, 248, 249);">处理请求</font>**<font style="background-color:rgb(247, 248, 249);">：控制器类中的方法通过映射注解（如 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">@GetMapping</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">@PostMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 等）处理 HTTP 请求。</font>

## <font style="background-color:rgb(247, 248, 249);">相关注解</font>
<font style="background-color:rgb(247, 248, 249);">在 Spring MVC 中，除了</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@Controller</font>`<font style="background-color:rgb(247, 248, 249);">，还有一些常用的注解用于处理请求：</font>

1. `**<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>**`<font style="background-color:rgb(247, 248, 249);">：用于定义请求 URL 和 HTTP 方法的映射。可以应用于类级别和方法级别。</font>
2. `**<font style="background-color:rgb(247, 248, 249);">@GetMapping</font>**`**<font style="background-color:rgb(247, 248, 249);">、</font>**`**<font style="background-color:rgb(247, 248, 249);">@PostMapping</font>**`**<font style="background-color:rgb(247, 248, 249);">、</font>**`**<font style="background-color:rgb(247, 248, 249);">@PutMapping</font>**`**<font style="background-color:rgb(247, 248, 249);">、</font>**`**<font style="background-color:rgb(247, 248, 249);">@DeleteMapping</font>**`<font style="background-color:rgb(247, 248, 249);">：分别用于处理 </font>`<font style="background-color:rgb(247, 248, 249);">GET</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">POST</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">PUT</font>`<font style="background-color:rgb(247, 248, 249);">、</font>`<font style="background-color:rgb(247, 248, 249);">DELETE</font>`<font style="background-color:rgb(247, 248, 249);"> 请求。是 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestMapping</font>`<font style="background-color:rgb(247, 248, 249);"> 的快捷方式。</font>
3. `**<font style="background-color:rgb(247, 248, 249);">@RequestParam</font>**`<font style="background-color:rgb(247, 248, 249);">：用于绑定请求参数到方法参数。可以指定参数名称、是否必需以及默认值。</font>
4. `**<font style="background-color:rgb(247, 248, 249);">@PathVariable</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用于绑定 URL 路径中的变量到方法参数。</font>

5. `**<font style="background-color:rgb(247, 248, 249);">@ModelAttribute</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用于将请求参数绑定到模型对象，并将模型对象添加到模型中。</font>

6. `**<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>**`<font style="background-color:rgb(247, 248, 249);">：</font>

<font style="background-color:rgb(247, 248, 249);">用于将方法的返回值直接作为 HTTP 响应体。常用于返回 JSON 或 XML 数据。</font>





> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/xhoz2vq2ztd5g2ke>