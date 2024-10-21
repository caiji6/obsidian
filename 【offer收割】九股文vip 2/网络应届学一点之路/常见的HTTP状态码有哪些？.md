# 👌常见的HTTP状态码有哪些？

### 1xx: 信息性响应
+ **100 Continue**：客户端应继续其请求。
+ **101 Switching Protocols**：服务器根据客户端的请求切换协议。

### 2xx: 成功
+ **200 OK**：请求成功，并且服务器返回了请求的资源。
+ **201 Created**：请求成功并且服务器创建了新的资源。
+ **202 Accepted**：请求已接受，但尚未处理完成。
+ **204 No Content**：请求成功，但没有内容返回。

### 3xx: 重定向
+ **301 Moved Permanently**：请求的资源已被永久移动到新的URL。
+ **302 Found**：请求的资源临时从不同的URL响应。
+ **303 See Other**：客户端应使用另一个URL获取资源。
+ **304 Not Modified**：资源未被修改，客户端可以使用缓存的版本。

### 4xx: 客户端错误
+ **400 Bad Request**：请求无效或格式错误。
+ **401 Unauthorized**：请求需要身份验证。
+ **403 Forbidden**：服务器拒绝请求，客户端没有权限访问资源。
+ **404 Not Found**：请求的资源不存在。
+ **405 Method Not Allowed**：请求方法不被允许。
+ **408 Request Timeout**：请求超时。
+ **409 Conflict**：请求与服务器的当前状态冲突。
+ **410 Gone**：请求的资源已永久删除。
+ **429 Too Many Requests**：客户端发送了太多请求。

### 5xx: 服务器错误
+ **500 Internal Server Error**：服务器内部错误。
+ **501 Not Implemented**：服务器不支持请求的方法。
+ **502 Bad Gateway**：服务器作为网关或代理，从上游服务器收到无效响应。
+ **503 Service Unavailable**：服务器暂时无法处理请求，通常是由于过载或维护。
+ **504 Gateway Timeout**：服务器作为网关或代理，未能及时从上游服务器获得响应。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/nykn0tlf004fhae0>