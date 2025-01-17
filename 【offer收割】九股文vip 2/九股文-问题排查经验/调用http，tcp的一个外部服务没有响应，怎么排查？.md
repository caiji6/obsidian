# 👌调用http，tcp的一个外部服务没有响应，怎么排查？

### 1. 检查网络连接
+ **Ping**：使用`ping`命令检查是否能连接到目标服务器。例如：

```plain
ping example.com
```

+ **Traceroute**：使用`traceroute`（在 Windows 上是`tracert`）命令检查到目标服务器的路径，看看是否有中间节点出现问题。例如：

```plain
traceroute example.com
```

### 2. 检查端口和防火墙
+ **端口检查**：使用`telnet`或`nc`（netcat）命令检查目标服务器的指定端口是否开放。例如：

```plain
telnet example.com 80
```

+ 或者：

```plain
nc -zv example.com 80
```

+ **防火墙**：检查本地和远程服务器的防火墙配置，确保允许所需的端口和协议。

### 3. 检查服务器状态
+ **服务器状态**：确保目标服务器正在运行并且服务正常。可以通过登录到服务器检查服务状态，或者使用监控工具查看服务器的运行状况。

### 4. 检查 DNS 配置
+ **DNS 解析**：使用`nslookup`或`dig`命令检查域名是否正确解析到 IP 地址。例如：或者：

```plain
nslookup example.com
```

```plain
dig example.com
```

### 5. 检查应用程序日志
+ **客户端日志**：检查客户端应用程序的日志，看看是否有任何错误信息或异常堆栈跟踪。
+ **服务器日志**：如果有权限，检查目标服务器的日志，看看是否有任何错误信息或异常。

### 6. 使用调试工具
+ **cURL**：使用`cURL`命令行工具发送 HTTP 请求并查看详细的请求和响应信息。例如：

```plain
curl -v http://example.com/data
```

+ **Postman**：使用 Postman 等工具发送 HTTP 请求，并查看请求和响应的详细信息。

### 7. 检查超时和重试策略
+ **超时配置**：确保客户端配置了合理的连接超时和读取超时。例如，使用 Apache HttpClient 时可以这样配置：

```plain
RequestConfigrequestConfig= RequestConfig.custom()
    .setConnectTimeout(5000)
    .setSocketTimeout(5000)
    .build();
```

+ **重试策略**：配置合理的重试策略，以应对临时的网络问题。例如，使用 Spring Retry 可以实现重试机制。

### 8. 检查负载均衡和代理
+ **负载均衡器**：如果使用了负载均衡器，检查其配置和状态，确保其正常工作。
+ **代理服务器**：如果通过代理服务器访问外部服务，检查代理服务器的配置和状态。

### 9. 联系服务提供商
+ **服务提供商**：如果以上步骤都没有发现问题，可以联系外部服务的提供商，询问他们是否有任何已知的服务中断或维护。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/hep86hewadnisl2e>