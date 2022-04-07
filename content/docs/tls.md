+++
date = "2017-11-17T11:45:24+08:00"
title = "TLS"
url = "tls"
weight = 30
+++

TLS是GOST支持的一种传输类型(Transport)。

## 使用说明

### 标准TLS

#### 服务端

```
gost -L tls://:443
```

#### 客户端

```
gost -L :8080 -F tls://server_ip:443
```

### 多路复用TLS

GOST在TLS基础之上扩展出具有多路复用(Multiplex)特性的TLS传输类型(mtls)。

#### 服务端

```
gost -L mtls://:443
```

#### 客户端

```
gost -L :8080 -F mtls://server_ip:443
```

## TLS证书

GOST内置了TLS证书，如果需要使用自定义TLS证书，有两种方法：

* 在GOST运行目录放置`cert.pem`(公钥)和`key.pem`(私钥)两个文件即可，GOST会自动加载运行目录下的cert.pem和key.pem文件。

* 使用`key`和`cert`参数指定证书文件路径：

```
gost -L="tls://:443?cert=/path/to/my/cert/file&key=/path/to/my/key/file"
```

## 证书校验

对于客户端可以通过`secure`参数开启服务器证书和域名校验，默认不校验证书:

```
gost -L=:8080 -F="tls://server_domain_name:443?secure=true"
```

当需要校验证书时，节点配置中的`server_domain_name`部分必须填写服务器的域名。

## 证书锁定

对于客户端可以通过`ca`参数指定CA证书进行[证书锁定](https://en.wikipedia.org/wiki/Transport_Layer_Security#Certificate_pinning)(Certificate Pinning):

```
gost -L=:8080 -F="tls://:443?ca=ca.pem"
```

{{< hint info >}}
以上参数可以用于所有支持TLS的服务，例如HTTP2, QUIC, WSS, SSH, SOCKS5。
{{< /hint >}}

## 双向证书校验(2.11.1+)

服务端可以通过`ca`参数指定CA证书来对客户端证书进行强制校验：

```
gost -L="tls://:443?cert=certfile&key=keyfile&ca=cafile"
```

此时客户端必须提供自己的证书：

```
gost -L=:8080 -F="tls://server_ip:443?cert=certfile&key=keyfile"
```
