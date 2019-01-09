+++
date = "2019-01-05T14:36:00+08:00"
menu = "main"
title = "探测防御"
weight = 22
+++

GOST在2.7版本中增加了对HTTP/HTTPS/HTTP2代理的探测防御功能。当代理服务收到非法请求时，会按照探测防御策略返回对应的响应内容。

{{< admonition title="注意" type="warning" >}}
只有当代理服务开启了用户认证，探测防御功能才有效。
{{< /admonition >}}

```bash
gost -L=http2://admin:123456@:443?probe_resist=code:400
```

代理服务通过`probe_resist`参数来指定防御策略。参数值的格式为：`type:value`。

type可选值有:

* `code` - 对应value为HTTP响应码，代理服务器会回复客户端指定的响应码。例如：`probe_resist=code:404`

* `web` - 对应的value为URL，代理服务器会使用HTTP GET方式访问此URL，并将响应返回给客户端。例如: `probe_resist=web:example.com/page.html`

* `host` - 对应的value为主机地址，代理服务器会将客户端请求转发给设置的主机地址，并将主机的响应返回给客户端，代理服务器在这里相当于端口转发服务。例如：`probe_resist=host:example.com:8080`

* `file` - 对应的value为本地文件路径，代理服务器会回复客户端200响应码，并将指定的文件内容作为Body发送给客户端。例如：`probe_resist=file:/path/to/file.txt`

此功能可应用于HTTP，HTTPS和HTTP/2代理：

```bash
gost -L=http://admin:123456@:8080?probe_resist=code:403
```

```bash
gost -L=https://admin:123456@:443?probe_resist=host:www.example.com:8080
```

```bash
gost -L=http2://admin:123456@:443?probe_resist=file:/send/to/client/file.txt
```
