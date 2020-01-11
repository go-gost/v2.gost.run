+++
date = "2017-11-20T12:33:00+08:00"
title = "KCP"
url = "kcp"
weight = 32
+++

KCP is a transport type supported by GOST.

Support for KCP is based on libraries [xtaci/kcp-go](https://github.com/xtaci/kcp-go) and [xtaci/kcptun](https://github.com/xtaci/kcptun).

Server:

```
gost -L=kcp://:8388
```

Client:

```
gost -L=:8080 -F=kcp://server_ip:8388
```

## Configuration

GOST has a default KCP configuration built-in，and is the same as xtaci/kcptun.

GOST will automatically load `kcp.json` configuration file from current working directory if exists, or you can use the parameter `c` to specify the path to the file.

```
gost -L=kcp://:8388?c=/path/to/conf/file
```

File format:

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

Please refer to the [kcptun](https://github.com/xtaci/kcptun#usage) for more detail.

{{< hint warning >}}
To use a KCP node in a proxy chain, there can be only one KCP node in the proxy chain, and this node can only serve as the first node in the proxy chain.
{{< /hint >}}

### Fake TCP (2.9.1+)

You can enable the fake TCP mode via `tcp` parameter or `tcp` option in the config file.

{{< hint warning >}}
This feature is only available on Linux.
{{< /hint >}}

#### Server side

```
gost -L=kcp://:8388?tcp=true
```

#### Client side

```
gost -L=:8080 -F=kcp://server_ip:8388?tcp=true
```
