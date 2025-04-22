# 关于xff伪造

伪造 HTTP 请求头中的 X-Forwarded-For（简称 XFF），用来隐藏真实 IP 或绕过基于 IP 的限制。

由于 X-Forwarded-For 是客户端可以自定义的 HTTP 头，攻击者可以轻松地在请求中添加或修改它，伪造任意 IP 地址。



# 场景

页面只允许本地访问

抓包，添加字段：

x-forwarded-for:127.0.0.1