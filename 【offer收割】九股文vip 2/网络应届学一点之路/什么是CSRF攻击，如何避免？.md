# 👌什么是 CSRF 攻击，如何避免？

跨站请求伪造（Cross-Site Request Forgery，简称 CSRF）是一种网络攻击，攻击者诱导受害者在已登录的情况下执行未经授权的操作。CSRF 攻击利用了用户已经认证的身份，冒充用户发送恶意请求，从而进行操作，如转账、修改设置等。

### CSRF 攻击原理
1. **受害者登录受信任网站 A，并保持会话（例如，通过 Cookie 认证）。**
2. **攻击者诱导受害者访问恶意网站 B。**
3. **恶意网站 B 发送一个请求到受信任网站 A，利用受害者的身份认证信息（如 Cookie），执行攻击者希望的操作。**

由于受信任网站 A 认为请求来自已认证的用户，因此会执行该请求。

### CSRF 攻击的危害
+ 伪造用户请求进行转账、支付等操作。
+ 修改用户账户设置、密码等。
+ 发起恶意请求，进行数据篡改或删除。

### 如何避免 CSRF 攻击
1. **使用 CSRF 令牌（CSRF Token）**：
    - 在每个受保护的请求中包含一个唯一的、不可预测的令牌。
    - 服务器验证令牌的有效性，确保请求是由合法用户发起的。
2. **同源策略（SameSite Cookie）**：
    - 设置 Cookie 的`SameSite`属性，限制 Cookie 在跨站请求中的发送。
    - `SameSite=Lax`：默认阻止跨站点的部分请求（如表单提交）。
    - `SameSite=Strict`：完全阻止跨站点的所有请求。
    - `SameSite=None; Secure`：允许跨站点请求，但要求使用 HTTPS。
3. **双重提交 Cookie（Double Submit Cookie）**：
    - 在请求中同时包含 CSRF 令牌作为 Cookie 和请求参数，服务器验证两者是否一致。
4. **验证请求来源（Referer 头）**：
    - 检查请求的`Referer`头，确保请求来源于受信任的域名。
    - 这种方法有局限性，某些情况下`Referer`头可能会被隐藏或修改。
5. **使用 HTTP 头（如**`**X-Requested-With**`**）**：
    - 对 AJAX 请求，检查`X-Requested-With`头，确保请求是通过 JavaScript 发起的。
    - 这种方法只能用于防御简单的 CSRF 攻击。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/qdodhd1tiuzq0rcs>