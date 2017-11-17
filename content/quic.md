+++
date = "2017-11-17T12:15:24+08:00"
menu = "main"
title = "QUIC"
weight = 33
+++

QUIC是GOST支持的一种传输类型(Transport)。GOST对QUIC的支持是基于[lucas-clemente/quic-go](https://github.com/lucas-clemente/quic-go)库。

服务端:

```bash
gost -L=quic://:6121
```

客户端:

```bash
gost -L=:8080 -F=quic://server_ip:6121
```

## 心跳

客户端可以通过`keepalive`参数开启心跳检测

```bash
gost -L=:8080 -F=quic://server_ip:6121?keepalive=true
```

{{< admonition title="注意" type="warning" >}}
若要在代理链中使用QUIC节点，则此代理链中只能有一个QUIC节点，且此节点只能作为代理链的第一个节点。
{{< /admonition >}}
