+++
date = "2017-11-15T16:50:24+08:00"
title = "Getting Started"
url = "getting-started"
weight = 5
+++

{{< hint info >}}
**Proxy Node**

In GOST，GOST and other proxy services are considered as proxy nodes, GOST can handle the requests itself, or forward the requests to any one or more proxy nodes.
{{< /hint >}}

## Run

### Start a service

![Figure 01](/img/001.png)

Start an HTTP / SOCKS5 proxy service listening on port 8080:

```
gost -L :8080
```

### Run multiple services

```
gost -L http2://:443 -L socks5://:1080 -L ss://aes-128-cfb:123456@:8338
```

### Use the forwarding proxy

![Figure 02](/img/002.png)

```
gost -L :8080 -F 192.168.1.1:8081
```

HTTP / SOCKS5 proxy service listening on port 8080, using 192.168.1.1:8081 as the proxy for forwarding.


### Multi-level forwarding proxy (proxy chain)

![Figure 03](/img/003.png)

```
gost -L=:8080 -F=quic://192.168.1.1:8081 -F=socks5+wss://192.168.1.2:8082 -F=http2://192.168.1.3:8083 ... -F=a.b.c.d:NNNN
```

GOST forwards the requests to a.b.c.d:NNNN through the proxy chain in the order set by `-F`, 

## Command line parameters

GOST currently has the following parameters:

> `-L` - Specify local server configuration, can set more than one.

> `-F` - Specify the forwarding server configuration, can set more than one to form a proxy chain.

> `-C` - Specify the external configuration file.

> `-D` - Enable debug mod, more detailed log output.

> `-V` - Show version。

### configuration file

In addition to configuring services directly from the command line, 
parameters can also be set by specifying the external configuration file with the `-C` parameter:

```
gost -C gost.json
```

The configuration file is standard `json` format:

```
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

Format description:

> `Debug` - Equivalent to command line parameter `-D`. (2.4+)

> `Retries` - The number of retries after a failed connection through the proxy chain.

> `ServeNodes` - Mandatory, Equivalent to command line parameter `-L`.

> `ChainNodes` - Equivalent to command line parameter `-F`.

> `Routes` - Optional. Service lists, each with an independent proxy chain.
