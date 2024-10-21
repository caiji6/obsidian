# 👌如何在 Spring MVC 中处理 JSON 数据？

# <font style="background-color:rgb(247, 248, 249);">题目详细答案</font>
<font style="background-color:rgb(247, 248, 249);">处理 JSON 数据主要涉及两个方面：接收 JSON 数据和返回 JSON 数据。</font>

## <font style="background-color:rgb(247, 248, 249);">接收 JSON 数据</font>
1. **<font style="background-color:rgb(247, 248, 249);">使用 </font>**`**<font style="background-color:rgb(247, 248, 249);">@RequestBody</font>**`**<font style="background-color:rgb(247, 248, 249);"> 注解</font>**<font style="background-color:rgb(247, 248, 249);">：在控制器方法的参数上使用 </font>`<font style="background-color:rgb(247, 248, 249);">@RequestBody</font>`<font style="background-color:rgb(247, 248, 249);"> 注解可以将 HTTP 请求体中的 JSON 数据自动解析为对应的 Java 对象。</font>

```plain
@PostMapping("/api/users")
public ResponseEntity<String> createUser(@RequestBody User user) {
    // 处理 user 对象
}
```

<font style="background-color:rgb(247, 248, 249);">在上面的例子中，</font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> 是一个普通的 Java 对象，Spring MVC 会自动将请求体中的 JSON 数据转换为 </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> 对象。</font>

2. **<font style="background-color:rgb(247, 248, 249);">手动解析 JSON</font>**<font style="background-color:rgb(247, 248, 249);">：如果你需要更多的控制权，可以使用 Jackson 或 Gson 等库手动解析 JSON 数据。首先，你需要将请求体读取到一个字符串中，然后使用 JSON 解析器将其转换为 Java 对象。例如，使用 Jackson：</font>

```plain
@PostMapping("/api/users")
public ResponseEntity<String> createUser(@RequestBody String json) {
    ObjectMapper mapper = new ObjectMapper();
    User user = mapper.readValue(json, User.class);
    // 处理 user 对象
}
```

## <font style="background-color:rgb(247, 248, 249);">返回 JSON 数据</font>
1. **<font style="background-color:rgb(247, 248, 249);">使用</font>****<font style="background-color:rgb(247, 248, 249);"> </font>**`**<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>**`**<font style="background-color:rgb(247, 248, 249);"> </font>****<font style="background-color:rgb(247, 248, 249);">注解</font>**<font style="background-color:rgb(247, 248, 249);">：在控制器方法上使用</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">@ResponseBody</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">注解可以将方法返回的对象自动序列化为 JSON 数据并写入到 HTTP 响应体中。例如：</font>

```plain
@GetMapping("/api/users/{id}")
@ResponseBody
public User getUser(@PathVariable Long id) {
    // 从数据库中获取用户
    return user;
}
```

<font style="background-color:rgb(247, 248, 249);">在上面的例子中，Spring MVC 会自动将</font><font style="background-color:rgb(247, 248, 249);"> </font>`<font style="background-color:rgb(247, 248, 249);">User</font>`<font style="background-color:rgb(247, 248, 249);"> </font><font style="background-color:rgb(247, 248, 249);">对象序列化为 JSON 数据并返回给客户端。</font>

2. **<font style="background-color:rgb(247, 248, 249);">手动序列化 JSON</font>**<font style="background-color:rgb(247, 248, 249);">：如果你需要更多的控制权，可以使用 Jackson 或 Gson 等库手动将 Java 对象序列化为 JSON 数据。例如，使用 Jackson：</font>

```plain
@GetMapping("/api/users/{id}")
public ResponseEntity<String> getUser(@PathVariable Long id) {
    // 从数据库中获取用户
    User user =...
    ObjectMapper mapper = new ObjectMapper();
    String json = mapper.writeValueAsString(user);
    return ResponseEntity.ok(json);
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/tqya2vcxpa8r9ax0>