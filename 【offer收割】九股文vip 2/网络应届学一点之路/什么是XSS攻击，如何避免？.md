# 👌什么是 XSS 攻击，如何避免？

跨站脚本攻击（Cross-Site Scripting，简称 XSS）是一种常见的网络安全漏洞，攻击者通过在网页中注入恶意脚本，使这些脚本在用户的浏览器中执行，从而窃取用户数据、劫持用户会话、重定向用户到恶意网站等。

### XSS 攻击类型
1. **反射型 XSS（Reflected XSS）**：
    - 恶意脚本嵌入在 URL 中，用户点击链接后，脚本被服务器响应并在用户浏览器中执行。
    - 例如，通过电子邮件或社交媒体发送恶意链接。
2. **存储型 XSS（Stored XSS）**：
    - 恶意脚本存储在服务器的数据库中，用户访问包含该脚本的页面时，脚本被加载并执行。
    - 例如，攻击者在留言板、评论区等位置提交恶意脚本。
3. **基于 DOM 的 XSS（DOM-based XSS）**：
    - 恶意脚本直接在客户端的 DOM 环境中执行，而不是通过服务器响应。
    - 例如，脚本通过修改页面的 DOM 元素来执行。

### XSS 攻击的危害
+ 窃取用户的 Cookie、会话令牌等敏感信息。
+ 劫持用户会话，冒充用户进行操作。
+ 重定向用户到恶意网站。
+ 在用户浏览器中执行任意代码，进行钓鱼攻击等。

### 如何避免 XSS 攻击
1. **输入验证和清理（Input Validation and Sanitization）**：
    - 对用户输入进行严格的验证和清理，确保输入数据的合法性。
    - 使用白名单验证，拒绝不符合要求的输入。
2. **输出编码（Output Encoding）**：
    - 在将用户输入输出到 HTML 页面时，对输出进行编码，防止恶意脚本被执行。
    - 使用合适的编码函数，如 HTML 实体编码、JavaScript 字符串编码、URL 编码等。
3. **使用安全的库和框架**：
    - 使用经过安全审计的库和框架，这些工具通常内置了防 XSS 的机制。
    - 例如，使用 React、Angular 等框架，它们在处理用户输入时会自动进行编码。
4. **内容安全策略（Content Security Policy，CSP）**：
    - 配置 CSP 头，限制页面可以加载的资源来源，防止加载恶意脚本。
    - 例如，只允许加载特定域名下的脚本和资源。
5. **避免使用**`**eval**`**、**`**innerHTML**`**等危险函数**：
    - 避免在代码中使用`eval`、`innerHTML`、`document.write`等容易引发 XSS 的函数。
    - 使用安全的替代方法，如`textContent`、`createElement`等。
6. **HTTPOnly 和 Secure Cookie**：
    - 设置 Cookie 的`HttpOnly`属性，防止 JavaScript 访问 Cookie。
    - 设置 Cookie 的`Secure`属性，确保 Cookie 只能通过 HTTPS 传输。
7. **定期安全审计和测试**：
    - 定期进行安全审计和渗透测试，发现并修复潜在的 XSS 漏洞。
    - 使用自动化工具扫描和检测 XSS 漏洞。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/rv720ed56itb2h7t>