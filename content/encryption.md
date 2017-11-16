+++
date = "2017-11-16T20:26:24+08:00"
menu = "main"
title = "加密机制"
weight = 15
+++

## HTTP

对于HTTP可以使用TLS加密整个通讯过程，即HTTPS代理：

服务端:

```bash
gost -L=https://:443
```

客户端:

```bash
gost -L=:8080 -F=https://server_ip:443
```

## HTTP2

GOST的HTTP2代理模式仅支持使用TLS加密的HTTP2协议，不支持明文HTTP2传输。

GOST的HTTP2隧道模式支持加密(h2)和明文(h2c)两种模式。

## SOCKS5

GOST支持标准SOCKS5协议的no-auth(0x00)和user-pass(0x02)方法，并在此基础上扩展了两个：tls(0x80)和tls-auth(0x82)，用于数据加密。

服务端:

```bash
gost -L=socks5://:1080
```

客户端:

```bash
gost -L=:8080 -F=socks5://server_ip:1080
```

如果两端都是GOST(如上)则数据传输会被加密(协商使用tls或tls-auth方法)，否则使用标准SOCKS5进行通讯(no-auth或user/pass方法)。

## Shadowsocks

gost对shadowsocks的支持是基于[shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)库。

服务端:

```bash
gost -L=ss://chacha20:123456@:8338
```

客户端:

```bash
gost -L=:8080 -F=ss://chacha20:123456@server_ip:8338
```

### Shadowsocks UDP relay

目前仅服务端支持UDP Relay。

服务端:

```bash
gost -L=ssu://chacha20:123456@:8338
```

## TLS

GOST内置了TLS证书，如果需要使用其他TLS证书，有两种方法：

* 在GOST运行目录放置`cert.pem`(公钥)和k`ey.pem`(私钥)两个文件即可，GOST会自动加载运行目录下的cert.pem和key.pem文件。

* 使用参数指定证书文件路径：

```bash
gost -L="http2://:443?cert=/path/to/my/cert/file&key=/path/to/my/key/file"
```

对于客户端可以通过`secure`参数开启服务器证书和域名校验，默认不校验证书:

```bash
gost -L=:8080 -F="http2://server_domain_name:443?secure=true"
```

对于客户端可以通过`ca`参数指定CA证书进行[证书锁定](https://en.wikipedia.org/wiki/Transport_Layer_Security#Certificate_pinning)(Certificate Pinning):

```bash
gost -L=:8080 -F="http2://:443?ca=ca.pem"
```

