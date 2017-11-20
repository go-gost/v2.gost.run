+++
date = "2017-11-17T11:10:24+08:00"
menu = "main"
title = "SOCKS"
weight = 21
+++

SOCKS是GOST支持的协议类型(Protocol)，SOCKS协议有三个版本: SOCKS4, SOCKS4A和SOCKS5。

## SOCKS4

```bash
gost -L socks4://:1080
```

标准的SOCKS4代理服务，同时兼容SOCKS4A协议。

## SOCKS4A

```bash
gost -L socks4a://:1080
```

标准的SOCKS4A代理服务。

{{< admonition title="注意" type="warning" >}}
SOCKS4(A)当前仅支持CONNECT方法，不支持BIND方法。
{{< /admonition >}}

## SOCKS5

```bash
gost -L socks5://:1080
```

### SOCKS5协商加密

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

### SOCKS5 UDP Relay

GOST中SOCKS5同时也支持UDP Relay，并支持TCP-over-UDP特性。

#### 不设置转发代理

![Figure 01](/gost/img/udp01.png)

GOST做为标准SOCKS5代理处理UDP数据。

#### 设置转发代理

![Figure 02](/gost/img/udp02.png)

#### 设置多个转发代理(代理链)

![Figure 03](/gost/img/udp03.png)

当设置转发代理时，GOST会使用UDP-over-TCP方式转发UDP数据。proxy1 - proxyN可以为任意类型代理。

{{< admonition title="注意" type="warning" >}}
如果要转发SOCKS5的BIND和UDP请求，代理链的末端(最后一个-F参数)必须是GOST SOCKS5类型代理。
{{< /admonition >}}

SOCKS协议也可以与各种传输类型(Transport)组合使用

### SOCKS5 Over TLS

```bash
gost -L socks5+tls://:1080
```

使用TLS加密的SOCKS5代理服务。

### SOCKS5 Over QUIC

```bash
gost -L socks5+quic://:1080
```