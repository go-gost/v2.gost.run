+++
date = "2018-5-19T21:15:00+08:00"
menu = "main"
title = "DNS控制"
weight = 90
+++

GOST在2.6版本中增加了自定义DNS服务功能，DNS控制可以应用于服务节点。每当此节点收到请求时，会使用此节点上指定的DNS服务对此请求的目标地址进行解析。

```bash
gost -L=:8080?dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls,https://1.0.0.1/dns-query
```

服务端通过`dns`参数来指定DNS服务列表(以逗号分割)，每个DNS服务的格式为：`ip[:port][/protocol]`，其中port默认为53，protocol可选值有：`udp`， `tcp`，`tls`，`https`。默认为`udp`。

也可以使用外部文件来指定DNS服务列表：

```bash
gost -L=:8080?dns=dns.txt
```

配置文件的格式为(按行分割的地址列表)：

```text
# options
timeout     5s
# ttl         60s
reload      10s

# ip[:port] [protocol] [hostname]
8.8.8.8
8.8.8.8     tcp
1.1.1.1     udp
1.1.1.1:53  tcp
1.1.1.1:853 tls     cloudflare-dns.com
https://1.0.0.1/dns-query
```

`timeout` - DNS请求超时时间，默认5秒。

`ttl` - DNS缓存有效期，默认使用DNS查询返回结果中的TTL。当设置为负值，则不使用缓存。

`reload` - 此配置文件支持热更新。此选项用来指定文件检查周期，默认关闭热更新。

DNS服务项分为三列：

第一列为DNS服务器地址，格式为ip[:port]，port默认为53.

第二列为协议类型，可选值有：`udp`， `tcp`，`tls`，`https`。 默认为`udp`。

第三列为DNS服务域名，当协议类型为`tls`时有效，用于TLS证书校验。

## 自定义域名解析

除了可以自定义DNS服务用来解析域名外，还可以手动指定域名-IP映射关系，类似于Linux下的/etc/hosts文件功能。

```bash
gost -L=:8080?hosts=hosts.txt
```

配置文件的格式：

```text
# options
reload  10s

# IP_address    canonical_hostname     [aliases...]
127.0.0.1       localhost
192.168.1.10    foo.mydomain.org       foo
192.168.1.13    bar.mydomain.org       bar baz
```

`reload` - 此配置文件支持热更新。此选项用来指定文件检查周期，默认关闭热更新。

映射表项分为三列：

第一列为IP地址。

第二列为主机名或域名。

第三列为别名，可以有多个。