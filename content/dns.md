+++
date = "2018-5-19T21:15:00+08:00"
menu = "main"
title = "DNS控制"
weight = 90
+++

GOST在2.6版本中增加了自定义DNS服务功能，DNS控制可以应用于服务节点。

```bash
gost -L=:8080?dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls
```

服务端通过`dns`参数来指定DNS服务列表(以逗号分割)，每个DNS服务的格式为：`ip[:port][/protocol]`，其中port默认为53，protocol可选值有：`udp`， `tcp`，`tls`，默认为`udp`。

也可以使用外部文件来指定DNS服务列表：

```bash
gost -L=:8080?dns=dns.txt
```

配置文件的格式为(按行分割的地址列表)：

```text
timeout     30
ttl         60

# ip[:port] [protocol] [hostname]
8.8.8.8
8.8.8.8     tcp
1.1.1.1     udp
1.1.1.1:53  tcp
1.1.1.1:853 tls     cloudflare-dns.com
```

`timeout` - DNS请求超时时间(秒)，默认30秒。

`ttl` - DNS缓存有效期(秒)，默认为60秒。当设置为负值，则不使用缓存。

DNS服务项分为三列：

第一列为DNS服务器地址，格式为ip[:port]，port默认为53.

第二列为协议类型，可选值有：`udp`， `tcp`，`tls`，默认为`udp`。

第三列为DNS服务域名，当协议类型为`tls`时有效，用于TLS证书校验。