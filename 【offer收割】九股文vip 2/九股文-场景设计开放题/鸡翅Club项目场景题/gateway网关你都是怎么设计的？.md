# 👌gateway 网关你都是怎么设计的？

gateway 网关，作为我们项目的整个流量入口，目前主要实现了路由，负载，统一鉴权，全局过滤器，异常处理这些功能。路由和负载承载了后台微服务的 uri 转发和前缀匹配。统一鉴权主要是配合 satoken，在 gateway 集成 redis，同时实现 satoken 提供的权限读取接口，在其中自定义读取逻辑，实现鉴权的校验。在其中还实现了登录拦截器，用于传递 loginId 到微服务中，借助了 header 的传递。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fwx0lmos1l9bo3fo>