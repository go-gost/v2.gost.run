+++
date = "2017-11-16T16:50:24+08:00"
menu = "main"
title = "节点配置"
weight = 10
+++

在GOST中一个代理服务逻辑上被分成两层：协议层(Protocol)和传输层(Transport)，两层之间相互独立，并可以任意组合使用。当GOST去连接一个代理节点时，会先进行传输层的交互，当传输层建立以后，再进行协议层的交互。

## 协议类型(Protocols)

目前支持的协议类型有：

* `http` - HTTP

* `socks4` - SOCKS4

* `socks4a` - SOCKS4A

* `socks5` - SOCKS5

* `ss` - Shadowsocks

* `sni` - SNI (2.5+)

* `forward` - Forward

## 传输类型(Transports)

目前支持的传输类型有：

* `tcp` - 原始TCP

* `ws` - Websocket

* `mws` - Multiplex Websocket，在Websocket上增加多路复用功能 (2.5+)

* `wss` - Websocket Secure，基于TLS加密的Websocket

* `mwss` - Multiplex Websocket Secure，在基于TLS加密的Websocket上增加多路复用功能 (2.5+)

* `tls` - TLS

* `mtls` - Multiplex TLS，在TLS上增加多路复用功能 (2.5+)

* `kcp` - KCP (2.3+)

* `quic` - QUIC (2.4+)

* `ssh` - SSH (2.4+)

* `h2` - HTTP2 (2.4+)

* `h2c` - HTTP2 Cleartext (2.4+)

* `obfs4` - OBFS4 (2.4+)

* `ohttp` - HTTP Obfuscation (2.5+)


## 配置格式

在GOST中节点的配置为类URL格式(适用于`-L`和`-F`参数)：

```bash
[scheme://][user:pass@host]:port[?param1=value1&param2=value2]
```

### **scheme** 

`scheme`可以是单独的协议类型或传输类型，或是二者的组合，也可以是空。

#### 不指定任何类型

不指定任何类型时，对于`-L`参数，协议层默认为是HTTP+SOCKS5，传输层默认为是原始TCP类型。对于`-F`参数，协议层默认为是HTTP类型，传输层默认为原始TCP类型。

```bash
gost -L :8080 -F :8888
```

#### 仅指定协议类型

当仅指定协议类型时，传输层默认为原始TCP类型。

```bash
gost -L http://:8080 -F socks5://:1080
```

#### 仅指定传输类型

当仅指定传输类型时，对于`-L`参数，协议类型默认为HTTP+SOCKS5。对于`-F`参数，协议层默认为是HTTP类型。

```bash
gost -L tls://:443 -F ws://:1443
```

#### 组合使用

```bash
gost -L http+tls://:443 -F socks5+wss://:1443
```

#### 特殊的scheme

除了上述的类型外，有几个比较特殊的sheme：

* `redirect` - TCP透明代理 (2.3+)

 ```bash
 gost -L redirect://:12345
 ```

* `ssu` - Shadowsocks UDP relay，目前仅服务端支持 (2.4+)。

```bash
gost -L ssu://chacha20:123456@:8338
```

* `tcp` - TCP本地端口转发 (2.1+)

```bash
gost -L tcp://:2222/:22
```

* `rtcp` - TCP远程端口转发 (2.1+)

```bash
gost -L rtcp://:2222/:22
```

* `udp` - UDP本地端口转发 (2.1+)

```bash
gost -L udp://:5353/192.168.1.1:53
```

* `rudp` - UDP远程端口转发 (2.1+)

```bash
gost -L rudp://:5353/192.168.1.1:53
```

### **节点认证**

`user:pass`用于指定服务的认证信息。对于shadowsocks，`user`为加密类型。

```bash
gost -L admin:123456@:8080 -F ss://chacha20:123456@:8338
```

对于HTTP/SOCKS5服务，也可以通过secrets参数来设定多组认证信息：

```bash
gost -L=:8080?secrets=secrets.txt
```

secrets.txt文件格式为按行分割的认证信息，每一行认证信息为用空格分割的user/pass对，以 `#` 开始的行为注释行。

```text
# username password

admin   123456
test\user001 123456
test.user@002 12345678
```