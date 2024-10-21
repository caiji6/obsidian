# 👌forward 和 redirect 的区别？

### Forward（转发）
`Forward`是服务器内部的请求转发。服务器接收到客户端的请求后，将请求转发给另一个资源（如Servlet、JSP等）进行处理，而客户端并不知道这一过程。

**特点**：

1. **URL不变**：客户端浏览器地址栏的URL不会改变，仍然是最初请求的URL。
2. **服务器内部操作**：请求和响应对象在服务器内部传递，不会发送新的HTTP请求。
3. **共享请求数据**：原始请求和响应对象可以在转发的资源之间共享，能够直接访问请求中的数据（如请求参数、属性等）。

**适用场景**：

+ 在同一个Web应用内部的资源之间转发请求。
+ 需要在多个资源之间共享请求数据时。

**示例**：

```plain
RequestDispatcher dispatcher = request.getRequestDispatcher("anotherResource");
dispatcher.forward(request, response);
```

### Redirect（重定向）
`Redirect`是客户端重定向。服务器接收到客户端的请求后，发送一个重定向响应（HTTP状态码302或其他重定向状态码）和新的URL给客户端，客户端浏览器会自动发起对新URL的请求。

**特点**：

1. **URL改变**：客户端浏览器地址栏的URL会改变为新的URL。
2. **新请求**：服务器发送重定向响应后，客户端会发起一个新的HTTP请求。
3. **请求数据不共享**：由于是新的请求，原始请求中的数据不会自动传递到新的请求中，除非通过URL参数或其他方式传递。

**适用场景**：

+ 导向不同的Web应用或外部资源。
+ 需要通知客户端浏览器进行新的请求，例如在用户登录后重定向到主页。

**示例**：

```plain
response.sendRedirect("newURL");
```

### 对比总结
1. **实现方式**：

`Forward`：服务器内部请求转发，不涉及客户端。

`Redirect`：服务器通知客户端进行新的请求。

1. **URL变化**：

`Forward`：URL不变。

`Redirect`：URL改变。

1. **请求数据**：

`Forward`：请求数据可以在转发的资源之间共享。

`Redirect`：请求数据不会自动传递，需要通过URL参数或其他方式传递。

1. **应用场景**：

`Forward`：适用于同一Web应用内部的资源之间的转发。

`Redirect`：适用于需要通知客户端浏览器进行新请求的情况，如跳转到外部资源或不同的Web应用。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/td2gzzi0rv78ruig>