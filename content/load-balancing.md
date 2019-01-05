+++
date = "2017-11-17T15:50:24+08:00"
menu = "main"
title = "负载均衡"
weight = 70
+++

{{< admonition title="节点组" type="note" >}}
一个节点组由一个或多个节点组成，每个节点可以是任意类型。
GOST代理链的每一层级都是一个节点组。
{{< /admonition >}}

GOST在2.5版本中增加了对负载均衡的支持。负载均衡可以应用于代理链的所有层级节点。

负载均衡有两种，这里简单的称之为简单型和复杂型，两种类型可以组合使用。

## 简单型

简单型是一种类似于DNS负载均衡的功能，可以为代理节点指定多个地址：

```bash
gost -L=:8080 -F='http://localhost:8080?ip=192.168.1.1,192.168.1.2:8081,192.168.1.3:8082&strategy=random' -F=socks5://localhost:1080?ip=172.20.1.1:1080,172.20.1.2:1081,172.20.1.3:1082
```

客户端通过`ip`参数来指定实际的代理服务地址(以逗号分割的列表)，地址格式可以是ip[:port]或hostname[:port]，若没有指定port则默认使用URL中的port。

{{< admonition title="注意" type="warning" >}}
当设置了`ip`参数，URL中指定的地址将会被忽略。
{{< /admonition >}}

通过`strategy`参数可以指定节点选择策略，默认为`round`(**注意: `strategy`参数需要2.6+版本**)。

![figure 01](../img/lb01.png)

当为一个层级指定了多个节点，GOST会将这些节点按指定顺序放在同一个节点组中。

每次客户端发送请求，代理链会先确定一条路径，对每一个节点组执行节点选择(随机或轮询)。

![figure 02](../img/lb02.png)

相当于将上面的命令转化为：

```bash
gost -L=:8080 -F=http://192.168.1.3:8082 -F=socks5://172.20.1.2:1081
```

若地址比较多，可以使用外部配置文件：

```bash
gost -L=:8080 -F=http://localhost:8080?ip=iplist1.txt -F=socks5://localhost:1080?ip=iplist2.txt
```

配置文件的格式为(按行分割的地址列表)：

```text
192.168.1.1
192.168.1.2:8081
192.168.1.3:8082
example.com:8083
```

在简单型中要求每一层级的所有节点的类型和配置保持一致。

## 复杂型

复杂型克服了简单型中的限制，可以自由指定代理链中每一层级节点组中节点的类型。

![figure 03](../img/lb03.png)

```bash
gost -L=:8080 -F=kcp://192.168.1.1:8388?peer=peer1.json -F=http2://172.20.1.1:443?peer=peer2.json
```

客户端通过`peer`参数指定额外的节点配置文件，配置文件格式为：

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

格式说明:

`strategy` - 指定节点选择策略，`round` - 轮询，`random` - 随机, `fifo` - 自上而下。默认值为`round`。

`max_fails` - 指定节点连接的最大失败次数，当与一个节点建立连接失败次数超过此设定值时，此节点会被标记为死亡节点(Dead)，死亡节点不会被选择使用。默认值为1。

`fail_timeout` - 指定死亡节点的超时时间，当一个节点被标记为死亡节点后，在此设定的时间间隔内不会被选择使用，超过此设定时间间隔后，会再次参与节点选择。默认为30秒。

`reload` - 此配置文件支持热更新。此选项用来指定文件检查周期，默认关闭热更新。

`peer` - 指定节点列表。

每次客户端发送请求，代理链会确定一条路径，对每一个节点组执行节点选择(随机或轮询)。

![figure 04](../img/lb04.png)

## 简单型+复杂型

组合使用时，代理链会将每一层级上(通过`ip`和`peer`参数)指定的所有节点放在同一个节点组中，再对每一个节点组执行节点选择(随机或轮询)，最终确定一条路径：

![figure 05](../img/lb05.png)

#### 完整的示例配置
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