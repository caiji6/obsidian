# 👌Session、Cookie和Token区别？

### 1. Session
**Session**是一种在服务器端保存用户状态信息的机制。每个用户在与服务器建立会话时，服务器会为其创建一个唯一的 Session ID，并将该 ID 存储在服务器端的会话存储中（例如内存、数据库或文件）。客户端通过 Cookie 或 URL 参数将 Session ID 发送到服务器，以便服务器可以识别用户并恢复其状态。

+ **存储位置**：服务器端。
+ **安全性**：较高，因为敏感数据存储在服务器端。
+ **生命周期**：通常在用户关闭浏览器或会话超时后失效。
+ **使用场景**：适用于需要在服务器端保持用户会话状态的场景，例如购物车、用户登录状态等。

### 2. Cookie
**Cookie**是一种在客户端（通常是浏览器）存储数据的小文件。服务器通过 HTTP 响应头`Set-Cookie`将 Cookie 发送到客户端，客户端会在后续请求中自动包含这些 Cookie。Cookie 可以用于存储用户的会话信息、偏好设置等。

+ **存储位置**：客户端（浏览器）。
+ **安全性**：较低，容易被窃取和篡改。可以通过设置`HttpOnly`和`Secure`属性提高安全性。
+ **生命周期**：可以设置为会话 Cookie（浏览器关闭后失效）或持久 Cookie（设置过期时间）。
+ **使用场景**：适用于需要在客户端存储少量数据的场景，例如用户偏好设置、跟踪用户活动等。

### 3. Token
**Token**是一种用于身份验证的字符串，通常由服务器生成并发送给客户端。Token 常用于无状态的身份验证机制，如 JSON Web Token (JWT)。客户端在每次请求时将 Token 发送到服务器，服务器通过验证 Token 来识别用户身份。

+ **存储位置**：客户端（可以存储在 Cookie、LocalStorage 或 SessionStorage 中）。
+ **安全性**：较高，Token 通常包含签名和加密信息，可以防止篡改。JWT 中的签名可以验证 Token 的完整性和真实性。
+ **生命周期**：可以设置过期时间，通常需要定期刷新 Token（如使用 Refresh Token）。
+ **使用场景**：适用于分布式系统和微服务架构中无状态的身份验证，特别是需要跨域的场景。

### 总结
+ **Session**：服务器端存储用户会话状态，通过 Session ID 识别用户。适用于需要在服务器端保持用户状态的场景。
+ **Cookie**：客户端存储少量数据，可以用于会话管理和用户偏好设置。安全性较低，需要注意保护敏感信息。
+ **Token**：客户端存储的身份验证字符串，常用于无状态的身份验证机制。适用于分布式系统和需要跨域的场景。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/qx8est2pcx0ak4xo>