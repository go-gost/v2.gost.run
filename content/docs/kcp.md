+++
date = "2017-11-17T12:05:24+08:00"
title = "KCP"
url = "kcp"
weight = 32
+++

KCP是GOST支持的一种传输类型(Transport)。

GOST对KCP的支持是基于[xtaci/kcp-go](https://github.com/xtaci/kcp-go)和[xtaci/kcptun](https://github.com/xtaci/kcptun)库。

服务端:

```
gost -L=kcp://:8388
```

客户端:

```
gost -L=:8080 -F=kcp://server_ip:8388
```

## 配置

GOST中内置了一套默认的KCP配置项，默认值与xtaci/kcptun中的一致。

GOST会自动加载当前工作目录中的kcp.json(如果存在)配置文件，或者可以手动通过参数`c`指定配置文件路径：

```
gost -L=kcp://:8388?c=/path/to/conf/file
```

配置文件格式：

```
{
    "key": "it's a secrect",
    "crypt": "aes",
    "mode": "fast",
    "mtu" : 1350,
    "sndwnd": 1024,
    "rcvwnd": 1024,
    "datashard": 10,
    "parityshard": 3,
    "dscp": 0,
    "nocomp": false,
    "acknodelay": false,
    "nodelay": 0,
    "interval": 40,
    "resend": 0,
    "nc": 0,
    "sockbuf": 4194304,
    "keepalive": 10,
    "snmplog": "",
    "snmpperiod": 60,
    "tcp": false
}
```

配置文件中的参数说明请参考[kcptun](https://github.com/xtaci/kcptun#usage)。

{{< hint warning >}}
若要在代理链中使用KCP节点，则此代理链中只能有一个KCP节点，且此节点只能作为代理链的第一个节点。
{{< /hint >}}

### Fake TCP (2.9.1+)

可以通过`tcp`参数或配置文件中的`tcp`项开启伪TCP模式。

{{< hint warning >}}
此功能仅适用于Linux。
{{< /hint >}}

#### 服务端

```
gost -L=kcp://:8388?tcp=true
```

#### 客户端

```
gost -L=:8080 -F=kcp://server_ip:8388?tcp=true
```
