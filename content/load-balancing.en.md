+++
date = "2017-11-20T14:00:00+08:00"
menu = "main"
title = "Load Balancing"
weight = 70
+++

{{< admonition title="Node Group" type="note" >}}
A node group consists of one or more nodes, each node can be any type.
In GOST, each level of the proxy chain is a node group.
{{< /admonition >}}

GOST added support for load balancing in version 2.5. Load balancing can be applied at all levels of the proxy chain.

There are two types of load balancing, here simply called **simple** and **complex**, the two types can be used in combination.

## Simple

Simple type is similar to DNS load balancing, you can specify multiple addresses for the proxy node:

```bash
gost -L=:8080 -F='http://localhost:8080?ip=192.168.1.1,192.168.1.2:8081,192.168.1.3:8082&strategy=round&max_fails=1&fail_timeout=30s' -F=socks5://localhost:1080?ip=172.20.1.1:1080,172.20.1.2:1081,172.20.1.3:1082
```

 **Options**

`ip` - the actual proxy address(es) (a comma-separated list), the address format can be ip[:port] or hostname[:port], If no port is specified, the port in the URL is used by default.

`strategy` - (2.6+) Specify node selection strategy, `round` for round-robin, `random` for random selection, `fifo` for top-down selection, the default is `round`.

`max_fails` - (2.8.1+) The maximum number of failed connections for a specified node, When the number of failed connections with a node exceeds this set value, the node will be marked as a **Dead node**, Dead node will not be selected to use. default value is 1.

`fail_timeout` - (2.8.1+) Specify the dead node's timeout period. When a node is marked as a dead node, it will not be selected within this set time interval. After this set time interval, it will participate in node selection again.

{{< admonition title="NOTE" type="warning" >}}
When the `ip` parameter is set, the address specified in the URL will be ignored.

When the `peer` (see below) parameter is set, the options above will be overwritten.
{{< /admonition >}}

The node selection strategy can be specified with the `strategy` parameter. Default value is `round` (**NOTE: `strategy` parameter requires 2.6+ version**).

![figure 01](/gost/img/lb01.png)

When more than one node is assigned to a level, GOST places the nodes in the same node group in the specified order.

Each time a client sends a request, the proxy chain first determines a path to perform node selection (random, round-robin or fifo) for each node group.

![figure 02](/gost/img/lb02.png)

After node selection, the command line is equivalent to:

```bash
gost -L=:8080 -F=http://192.168.1.3:8082 -F=socks5://172.20.1.2:1081
```

If there are too many addresses, you can use the configuration file:

```bash
gost -L=:8080 -F=http://localhost:8080?ip=iplist1.txt -F=socks5://localhost:1080?ip=iplist2.txt
```

The configuration file format is a list of addresses separated by new-line:

```text
192.168.1.1
192.168.1.2:8081
192.168.1.3:8082
example.com:8083
```

The simple type requires that the type and configuration of all nodes at each level be consistent.

## Complex

Complex type overcomes the limitations of the simple type and gives you the freedom to specify the type of node in each node group in the proxy chain.

![figure 03](/gost/img/lb03.png)

```bash
gost -L=:8080 -F=kcp://192.168.1.1:8388?peer=peer1.json -F=http2://172.20.1.1:443?peer=peer2.json
```

The client specifies additional node configuration file via the `peer` parameter. The configuration file format is:

```text
# strategy for node selecting
strategy        random

max_fails       1

fail_timeout    30s

# period for live reloading
reload          10s

# peers
peer    http://:18080
peer    socks://:11080
peer    ss://chacha20:123456@:18338
```

Format description:

`strategy` - Same as the `strategy` option.

`max_fails` - Same as the `max_fails` option.

`fail_timeout` - Same as the `fail_timeout` option.

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

`peer` - Specify the peer node list.

Each time a client sends a request, the proxy chain first determines a path to perform node selection (random or round-robin) for each node group.

![figure 04](/gost/img/lb04.png)

## Simple + Complex

When used in combination, the proxy chain places all nodes specified at each level (via the `ip` and `peer` parameters) in the same node group and performs node selection (random or round-robin) on each node group, Finally determine a path:

![figure 05](/gost/img/lb05.png)

#### Full example configuration
*gost.json*:
```json
{
    "Debug": false,
    "Retries": 3,
    "Routes": [
        {
            "Retries": 3,
            "ServeNodes": [
                ":8888"
            ],
            "ChainNodes": [
                ":1080?peer=peer.json"
            ]
        }
    ]
}
```
*peer.json*:
```json
{
    "strategy": "fifo",
    "max_fails": 1,
    "fail_timeout": 86400,
    "nodes": [
        "ss+kcp://aes-128-cfb:pass@[host]:port?ip=ips.txt"
    ]
}
```
*ips.txt*:
```txt
host1[:port]
host2[:port]
host3[:port]
```

