+++
date = "2017-11-17T11:16:24+08:00"
title = "Shadowsocks"
url = "ss"
weight = 24
+++

Shadowsocks是GOST支持的一种协议类型(Protocol)。

GOST对shadowsocks的支持是基于[shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)库。

## 使用说明
---

{{< hint warning >}}
从2.10.1+开始加密变为可选。
{{< /hint >}}

### TCP

#### 参数说明

`nodelay` - true/false (默认值为false)。默认情况下ss协议会等待客户端的请求数据，当收到请求数据后会把协议头部信息与请求数据一起发给服务端。当此参数设为`true`后，协议头部信息会立即发给服务端，不再等待客户端的请求。

##### 服务端

```
gost -L=ss://chacha20:password@:8338
```

##### 客户端

```
gost -L=:8080 -F=ss://chacha20:password@server_ip:8338?nodelay=true
```

#### AEAD加密

{{< hint warning >}}
2.10.1+后，`ss2`功能已经合并到`ss`中，可以直接在`ss`中使用AEAD加密，`ss2`已废弃。
{{< /hint >}}

在2.8版本中，GOST基于[shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2)增加了对AEAD加密的支持。

##### 服务端

```
gost -L=ss2://AEAD_CHACHA20_POLY1305:password@:8338
```

##### 客户端

```
gost -L=:8080 -F=ss2://AEAD_CHACHA20_POLY1305:password@server_ip:8338
```

#### 组合传输层

Shadowsocks协议可以与各种传输类型(Transport)组合使用

##### Shadowsocks Over TLS

```
gost -L ss+tls://chacha20:123456@:8338
```

##### Shadowsocks Over KCP

```
gost -L ss+kcp://chacha20:123456@:8338
```

### UDP

##### 服务端

```
gost -L=ssu://method:password@:8338?ttl=60s
```

`ttl` - (2.10+) 传输通道超时时间，默认为60s。

## 2.10+后的变化
---

### 客户端支持

`ssu`可以用在转发链中用来转发UDP数据：

```
gost -L udp://:5353/8.8.8.8:53 -F=ssu://method:password@:8338
```

### 加密方法

加密变为可选。

基于[shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2)增加了对AEAD加密的支持，且兼容老版本中的加密方式。

当设置了加密，会优先使用(shadowsocks/shadowsocks-go)库，若加密方法不支持，则切换到(shadowsocks/go-shadowsocks2)库。

### 组合传输层

`ssu`在2.10版本中，变成了一种协议类型，可以与各种传输类型组合使用：

```
gost -L ssu+kcp://:8338
```

```
gost -L ssu+wss://:443
```

默认情况下`ssu`等同于`ssu+udp`，传输层为原始UDP协议。这种情况下，GOST会使用shadowsocks本身的UDP relay协议进行数据转发。

如果指定了其他传输类型，则会使用SOCKS5的UDP relay协议进行数据转发。

{{< hint warning >}}
`ssu`仅能用于转发UDP数据，转发TCP数据的行为未定义。

当用在转发链中时，ssu必须为转发链的最后一个节点。

当`ssu+udp`用于转发链中时，转发链中不能存在其他节点。

{{< /hint >}}