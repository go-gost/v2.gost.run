+++
date = "2017-11-15T16:50:24+08:00"
menu = "main"
title = "快速开始"
+++

## 启动运行

### 开启代理服务

![Figure 01](/gost/img/001.png)

启动一个监听在8080端口的HTTP/SOCKS5代理服务：

```bash
gost -L :8080
```

### 开启多个代理服务

```bash
gost -L http2://:443 -L socks5://:1080 -L ss://aes-128-cfb:123456@:8338
```

### 使用转发代理

![Figure 02](/gost/img/002.png)

```bash
gost -L :8080 -F 192.168.1.1:8081
```

监听在8080端口的HTTP/SOCKS5代理服务，使用192.168.1.1:8081作为上级代理进行转发。


### 使用多级转发代理(代理链)

![Figure 03](/gost/img/003.png)

```bash
gost -L=:8080 -F=quic://192.168.1.1:8081 -F=socks5+wss://192.168.1.2:8082 -F=http2://192.168.1.3:8083 ... -F=a.b.c.d:NNNN
```

GOST按照`-F`设置的顺序通过代理链将请求最终转发给a.b.c.d:NNNN处理。

{{< admonition title="代理节点" type="note" >}}
在GOST中，GOST与其他代理服务都被看作是代理节点，GOST可以自己处理请求，或者将请求转发给任意一个或多个代理节点。
{{< /admonition >}}

## 启动参数

GOST目前有以下几个参数项：

> `-L` - 指定本地服务配置，可设置多个。

> `-F` - 指定转发服务配置，可设置多个，构成转发链。

> `-C` - 指定外部配置文件。

> `-D` - 开启Debug模式，更详细的日志输出。

> `-V` - 查看版本，显示当前运行的gost版本号。

### 配置文件

除了通过命令行直接配置服务外，也可以通过`-C`参数指定外部配置文件来设置参数：

```bash
gost -C gost.json
```

配置文件为标准`json`格式：

```json
{
    "Debug": true,
    "Retries": 0,
    "ServeNodes": [
        ":8080",
        "ss://chacha20:12345678@:8338"
    ],
    "ChainNodes": [
        "http://192.168.1.1:8080",
        "https://10.0.2.1:443"
    ],
    "Routes": [
        {
            "Retries": 1,
            "ServeNodes": [
                "ws://:1443"
            ],
            "ChainNodes": [
                "socks://:192.168.1.1:1080"
            ]
        },
        {
            "Retries": 3,
            "ServeNodes": [
                "quic://:443"
            ]
        }
    ]
}
```

格式说明：

> `Debug` - 对应命令行参数`-D`。

> `Retries` - 通过代理链建立连接失败后，重试次数。此功能需要2.5及以上版本。

> `ServeNodes` - 等同于命令行参数`-L`。

> `ChainNodes` - 等同于命令行参数`-F`。

> `Routes` - 额外的服务列表，每一项都拥有独立的转发链。此功能需要2.5及以上版本。
