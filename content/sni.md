+++
date = "2017-11-17T11:30:24+08:00"
menu = "main"
title = "SNI"
weight = 25
+++

SNI是GOST支持的一种协议类型(Protocol)。

服务端:

```bash
gost -L sni://:443
```

客户端:

可以通过配置hosts直接使用，或使用GOST进行转发:

```bash
gost -L :8080 -F sni://server_ip:443
```

## Host混淆

在GOST中SNI客户端可以通过`host`参数来指定Host别名：

```bash
gost -L :8080 -F sni://server_ip:443?host=example.com
```

SNI客户端会将TLS握手或HTTP请求头中的Host替换为`host`参数指定的内容。

SNI协议可以与各种传输类型(Transport)组合使用

### SNI Over TLS

```bash
gost -L sni+tls://:443
```

### SNI Over Websocket

```bash
gost -L sni+ws://:443
```