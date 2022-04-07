+++
date = "2017-11-17T15:11:24+08:00"
title = "HTTP2"
url = "http2"
weight = 21
+++

在GOST中HTTP2有两种模式：代理模式和隧道模式。

## 代理模式

在代理模式中，HTTP2被用作协议类型，传输类型必须为空。

```
gost -L http2://:443
```

443端口就是一个标准的HTTP2代理服务。

{{< hint warning>}}
代理模式下仅支持使用TLS加密的HTTP2协议，不支持明文HTTP2传输。
{{< /hint >}}

## 隧道模式

在隧道模式中，HTTP2被用作传输类型：`h2`, `h2c`。

```
gost -L socks5+h2://:443
```

或

```
gost -L http+h2c://:443
```

隧道模式下支持加密(h2)和明文(h2c)两种模式。

### 参数说明

`path` - (2.9+) 可选，指定HTTP请求URI。 