+++
date = "2018-5-19T21:15:00+08:00"
title = "DNS解析"
url = "resolver"
weight = 90
+++

GOST在2.6版本中可以自定义DNS解析。DNS解析应用于服务节点。

每当节点收到请求时，会使用此节点上指定的DNS解析器对此请求的目标地址进行解析。

## 使用说明

```
gost -L=http://:8080?dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls,https://1.0.0.1/dns-query
```

### 参数项

`dns` - 指定DNS服务列表(以逗号分割)，每个DNS服务的格式为：`ip[:port][/protocol]`，其中port默认为53，protocol可选值有：`udp`，`udp-chain`，`tcp`，`tcp-chain`，`tls`，`tls-chain`，`https`，`https-chain`。默认值为`udp`。

### 代理链(2.10.0+)

如果需要通过代理链来连接DNS服务器，则可以使用以下protocols:

`udp-chain`, `tcp-chain`, `tls-chain`, `https-chain`

```
gost -L=http://:8080?dns=8.8.8.8/udp-chain,1.1.1.1:853/tls-chain,https-chain://dns.google/dns-query -F=socks://:1080
```

{{< hint warning >}}
如果要转发`udp-chain`，代理链的末端(最后一个-F参数)必须是GOST `socks5`或`ssu`类型代理。
{{< /hint >}}

### 配置文件

也可以使用外部文件来指定DNS服务列表：

```
gost -L=:8080?dns=dns.txt
```

配置文件的格式为(按行分割的地址列表)：

```
# options
timeout     5s
# ttl       60s
reload      10s

# prefer    ipv6

# ip        1.2.3.4

# ip[:port] [protocol] [hostname]
8.8.8.8
8.8.8.8     tcp
1.1.1.1     udp
1.1.1.1:53  tcp-chain
1.1.1.1:853 tls     cloudflare-dns.com
https://1.0.0.1/dns-query   https-chain
```

`timeout` - DNS请求超时时间，默认5秒。

`ttl` - DNS缓存有效期，默认使用DNS查询返回结果中的TTL。当设置为负值，则不使用缓存。

`reload` - 此配置文件支持热更新。此选项用来指定文件检查周期，默认关闭热更新。

`prefer` - (2.8.2+) AAAA(IPv6)优先于A(IPv4)。

`ip` - 2.10.1+，客户端IP，设置后会开启ECS(EDNS Client Subnet)扩展功能。

DNS服务项分为三列：

第一列为DNS服务器地址，格式为ip[:port]，port默认为53.

第二列为协议类型，可选值有：`udp`，`udp-chain`，`tcp`，`tcp-chain`，`tls`，`tls-chain`，`https`，`https-chain`。 默认为`udp`。

第三列为DNS服务域名，当协议类型为`tls`，`tls-chain`，`https`，`https-chain`时有效，用于TLS证书校验。

## 自定义域名解析

除了可以自定义DNS服务用来解析域名外，还可以手动指定域名-IP映射关系，类似于Linux下的/etc/hosts文件功能。

```
gost -L=:8080?hosts=hosts.txt
```

配置文件的格式：

```
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
