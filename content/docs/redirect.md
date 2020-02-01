+++
date = "2017-11-17T15:40:24+08:00"
title = "透明代理"
url = "redirect"
weight = 40
+++

GOST在2.3版本中增加了对TCP透明代理的支持，在2.10版本中增加了对UDP透明代理的支持。

{{< hint warning >}}
透明代理仅支持Linux系统。
{{< /hint >}}

## TCP

```
gost -L red://:12345 -F 192.168.1.1:1080
```

### 本地全局TCP代理：

设置iptables：

```
iptables -t nat -A OUTPUT -p tcp --match multiport ! --dports 12345,1080 -j DNAT --to-destination 127.0.0.1:12345
```

## UDP (2.10+)

UDP透明代理是基于iptables的tproxy模块实现的。

```
gost -L redu://:12345?ttl=60s -F ssu://1.2.3.4:1080
```

`ttl` - (2.10+) 传输通道超时时间，默认为60s。

### 本地全局UDP代理

#### 设置iptables规则

{{< hint warning >}}
规则中的`192.168.0.0/16`为本机所在网络，`1.2.3.4/32`为转发服务端地址，请根据实际情况进行修改。
{{< /hint >}}

```
iptables -t mangle -N GOST
iptables -t mangle -N GOST_LOCAL

iptables -t mangle -A GOST -d 255.255.255.255/32 -j RETURN
iptables -t mangle -A GOST -d 127.0.0.0/8 -p udp -j RETURN
iptables -t mangle -A GOST -d 192.168.0.0/16 -p udp -j RETURN
iptables -t mangle -A GOST -p udp -j TPROXY --on-port 12345 --on-ip 0.0.0.0 --tproxy-mark 1

iptables -t mangle -A GOST_LOCAL -d 255.255.255.255/32 -j RETURN
iptables -t mangle -A GOST_LOCAL -d 192.168.0.0/16 -p udp -j RETURN
iptables -t mangle -A GOST_LOCAL -d 1.2.3.4/32 -p udp -j RETURN
iptables -t mangle -A GOST_LOCAL -p udp -j MARK --set-mark 1

iptables -t mangle -A PREROUTING -j GOST
iptables -t mangle -A OUTPUT -j GOST_LOCAL
```

#### 设置路由表

```
ip rule add fwmark 1 table 100
ip route add local 0.0.0.0/0 dev lo table 100
```