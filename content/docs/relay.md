+++
date = "2020-02-26T17:30:00+08:00"
title = "Relay"
url = "relay"
weight = 55
+++

Relay是GOST(2.11+)支持的一种协议类型(Protocol)。

Relay协议同时具有代理和转发功能，可同时处理TCP和UDP的数据，并支持用户认证。

{{< hint warning >}}
Relay协议本身不具备加密功能，如果需要对数据进行加密传输，可以配合加密隧道使用。
{{< /hint >}}

## 代理功能

Relay协议可以像HTTP/SOCKS5一样用作代理协议。

##### 服务端

```
gost -L relay+tls://username:password@:12345
```

##### 客户端

```
gost -L :8080 -F relay+tls://username:password@:12345
```

## 转发功能

Relay转发有两种模式，一种是配合端口转发使用，另一种是配合转发隧道使用。两种模式均可以同时转发TCP和UDP数据。

### 端口转发

##### 服务端

```
gost -L relay://:12345
```

##### 客户端

```
gost -L udp://:1053/:53 -L tcp://:1053/:53 -F relay://:12345
```

### 转发隧道

##### 服务端

```
gost -L relay://:12345/:53
```

##### 客户端

```
gost -L udp://:53 -L tcp://:53 -F relay://:12345
```