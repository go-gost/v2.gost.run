+++
date = "2017-11-16T20:26:24+08:00"
title = "权限控制"
url = "permission"
weight = 60
+++

服务端可以通过白名单`whitelist`参数和黑名单`blacklist`参数来控制客户端的请求是否允许被处理。
参数格式为: `[actions]:[hosts]:[ports]`

`[actions]`是一个由`,`分割的动作列表，可选值有: `tcp`(TCP转发), `udp`(UDP转发), `rtcp`(TCP远程转发), `rudp`(UDP远程转发), 或 `*`(所有动作)。

`[hosts]`是一个由`,`分割的Host列表，代表可以绑定到(rtcp,rudp)或转发到(tcp,udp)的目的主机，支持通配符(*.google.com)和`*`(所有主机)。

`[ports]`是一个由`,`分割的端口列表，代表可以绑定到(rtcp,rudp)或转发到(tcp,udp)的目的端口，可以是`*`(所有端口)。

多组权限可以通过`+`进行连接:

`whitelist=rtcp,rudp:localhost,127.0.0.1:2222,8000-9000+udp:8.8.8.8,8.8.4.4:53`(允许TCP/UDP远程端口转发绑定到localhost,127.0.0.1的2222端口和8000-9000端口范围，同时允许UDP转发到8.8.8.8:53和8.8.4.4:53)。

SSH远程端口转发只能绑定到127.0.0.1:8000

```
gost -L=forward+ssh://localhost:8389?whitelist=rtcp:127.0.0.1:8000
```

SOCKS5的TCP/UDP远程端口转发只允许绑定到大于1000的端口

```
gost -L=socks://localhost:8389?blacklist=rtcp,rudp:*:0-1000
```

SOCKS5的UDP转发只能转发到8.8.8.8:53

```
gost -L=socks://localhost:8389?whitelist=udp:8.8.8.8:53
```
