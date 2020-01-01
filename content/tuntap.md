+++
date = "2019-12-26T15:33:00+08:00"
menu = "main"
title = "TUN/TAP设备(Beta)"
weight = 100
+++

GOST在2.9版本中增加了对TUN/TAP设备的支持。基于TUN/TAP设备可以简单的构建VPN。

{{< admonition title="注意" type="warning" >}} 
此功能目前处于测试阶段，不能保证功能的完善和稳定性，请谨慎使用。

目前仅支持IPv4协议。
{{< /admonition >}}


# Windows

Windows下需要安装tap驱动后才能使用，可以选择安装[OpenVPN/tap-windows6](https://github.com/OpenVPN/tap-windows6)或[OpenVPN client](https://github.com/OpenVPN/openvpn)。


# TUN

## 使用说明

```
gost -L="tun://[method:password@][local_ip]:port[/remote_ip:port]?net=192.168.123.1/24&name=tun0&mtu=1350&route=10.100.0.0/16"
```

`method:password` - 指定UDP隧道数据加密方法和密码。所支持的加密方法与[shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2)一致。

`local_ip:port` - 本地监听的UDP隧道地址。

`remote_ip:port` - 可选，目标UDP地址。本地TUN设备收到的IP包会通过UDP转发到此地址。

`net` - 必须，指定TUN设备的地址。

`name` - 可选，指定TUN设备的名字，默认值为系统预设。

`mtu` - 可选，设置TUN设备的MTU值，默认值为1350。

`route` - 可选，逗号分割的路由列表:，例如：`10.100.0.0/16,172.20.1.0/24,1.2.3.4/32`

## 构建基于TUN设备的VPN (Linux)

### 创建TUN设备并建立UDP隧道

**注意：** `net`所指定的地址可能需要根据实际情况进行调整。

#### 服务端

```
gost -L tun://:8421?net=192.168.123.1/24
```

#### 客户端

```
gost -L tun://:8421/SERVER_IP:8421?net=192.168.123.2/24
```

当以上命令运行无误后，可以通过`ip addr`命令来查看创建的TUN设备：
```
$ ip addr show tun0
2: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1350 qdisc pfifo_fast state UNKNOWN group default qlen 500
    link/none 
    inet 192.168.123.2/24 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::d521:ad59:87d0:53e4/64 scope link flags 800 
       valid_lft forever preferred_lft forever
```

可以通过在客户端执行`ping`命令来测试一下隧道是否连通：
```
$ ping 192.168.123.1
64 bytes from 192.168.123.1: icmp_seq=1 ttl=64 time=9.12 ms
64 bytes from 192.168.123.1: icmp_seq=2 ttl=64 time=10.3 ms
64 bytes from 192.168.123.1: icmp_seq=3 ttl=64 time=7.18 ms
```

如果能ping通，说明隧道已经成功建立。


### iperf3测试

#### 服务端：

```
$ iperf3 -s
```

#### 客户端

```
$ iperf3 -c 192.168.123.1
```

### 路由规则和防火墙设置

如果想让客户端访问到服务端的网络，还需要根据需求设置相应的路由和防火墙规则。例如可以将客户端的所有外网流量转发给服务端处理

#### 服务端

开启IP转发并设置防火墙规则

```
$ sysctl -w net.ipv4.ip_forward=1

$ iptables -t nat -A POSTROUTING -s 192.168.123.0/24 ! -o tun0 -j MASQUERADE
$ iptables -A FORWARD -i tun0 ! -o tun0 -j ACCEPT
$ iptables -A FORWARD -o tun0 -j ACCEPT
```

#### 客户端

设置路由规则

**注意：**以下操作会更改客户端的网络环境，除非你知道自己在做什么，请谨慎操作！

```
$ ip route add SERVER_IP/32 via eth0   # 请根据实际情况替换SERVER_IP和eth0
$ ip route del default   # 删除默认的路由
$ ip route add default via 192.168.123.2  # 使用新的默认路由
```

# TAP

**注意：目前不支持MacOS。** 

## 使用说明

```
gost -L="tap://[method:password@][local_ip]:port[/remote_ip:port]?net=192.168.123.1/24&name=tap0&mtu=1350&route=10.100.0.0/16"
```

# 基于TCP的TUN/TAP隧道

GOST中的TUN/TAP隧道是基于UDP协议进行数据传输。如果想采用TCP传输，可以通过端口转发来实现

### 服务端

```
gost -L tun://:8421?net=192.168.123.1/24 -L socks5://:1080
```

### 服务端

```
gost -L tun://:8421/:8420?net=192.168.123.2/24 -L udp://:8420/:8421 -F socks5://server_ip:1080
```