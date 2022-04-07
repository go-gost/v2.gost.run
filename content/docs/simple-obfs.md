+++
date = "2017-11-17T15:30:24+08:00"
title = "Simple-obfs"
url = "simple-obfs"
weight = 35
+++

Simple-obfs是GOST支持的一种传输类型(Transport)。

Simple-obfs兼容[shadowsocks/simple-obfs](https://github.com/shadowsocks/simple-obfs)和Android上的[Simple Obfuscation](https://play.google.com/store/apps/details?id=com.github.shadowsocks.plugin.obfs_local)插件。

## 使用说明

### obfs-http

##### 服务端

```
gost -L=ss+ohttp://chacha20:123456@:8338
```

##### 客户端

```
gost -L=:8080 -F=ss+ohttp://chacha20:123456@server_ip:8338?host=bing.com
```

客户端可以通过`host`参数自定义请求Host。

### obfs-tls (2.11+)

##### 服务端

```
gost -L=ss+otls://chacha20:123456@:8338
```

##### 客户端

```
gost -L=:8080 -F=ss+otls://chacha20:123456@server_ip:8338?host=bing.com
```

客户端可以通过`host`参数自定义请求Host。


{{< hint warning >}}
obfs-tls目前仅支持[shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)库所支持的加密方式，不支持AEAD加密。
{{< /hint >}}