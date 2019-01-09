+++
date = "2017-11-17T11:16:24+08:00"
menu = "main"
title = "Shadowsocks"
weight = 24
+++

Shadowsocks是GOST支持的一种协议类型(Protocol)。

GOST对shadowsocks的支持是基于[shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)库。

{{< admonition title="加密" type="note" >}}
使用shadowsocks时必须指定加密方法和密码。
{{< /admonition >}}

服务端:

```bash
gost -L=ss://chacha20:123456@:8338
```

客户端:

```bash
gost -L=:8080 -F=ss://chacha20:123456@server_ip:8338
```

## Shadowsocks UDP relay

目前仅服务端支持UDP Relay。

服务端:

```bash
gost -L=ssu://chacha20:123456@:8338
```

Shadowsocks协议可以与各种传输类型(Transport)组合使用

## Shadowsocks Over TLS

```bash
gost -L ss+tls://chacha20:123456@:8338
```

## Shadowsocks Over KCP

```bash
gost -L ss+kcp://chacha20:123456@:8338
```
