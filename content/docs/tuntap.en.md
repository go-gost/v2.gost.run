+++
date = "2020-01-07T14:58:00+08:00"
title = "TUN/TAP Device"
url = "tuntap"
weight = 100
+++

GOST added support for TUN/TAP devices in version 2.9. You can create VPN via TUN/TAP devices.

## Windows

You need to install the tap driver [OpenVPN/tap-windows6](https://github.com/OpenVPN/tap-windows6) or [OpenVPN client](https://github.com/OpenVPN/openvpn) for Windows.

## TUN

### Usage

```
gost -L="tun://[method:password@][local_ip]:port[/remote_ip:port]?net=192.168.123.2/24&name=tun0&mtu=1350&route=10.100.0.0/16&gw=192.168.123.1"
```

`method:password` - Optional, encryption method and password for UDP tunnel. Supported methods are the same as [shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2).

`local_ip:port` - Local UDP tunnel listen address.

`remote_ip:port` - Optional, remote UDP server address, IP packets received by the local TUN device will be forwarded to the remote server via UDP tunnel.

`net` - Required, CIDR IP address of the TUN device, such as: 192.168.123.1/24.

`name` - Optional, TUN device name.

`mtu` - Optional, MTU for TUN device. Default value is 1350.

`route` - Optional, comma-separated routing items, such as: 10.100.0.0/16,172.20.1.0/24,1.2.3.4/32

`gw` - Optional, routing gateway.

`tcp` - Optional, use fake TCP tunnel, default value is `false` means UDP-based tunnel.

### TUN-based VPN (Linux)


{{< hint warning >}} 
The value specified by `net` option may need to be adjusted according to your actual situation.
{{< /hint >}}

#### Create a TUN device and establish a UDP tunnel

##### Server side

```
gost -L tun://:8421?net=192.168.123.1/24
```

##### Client side

```
gost -L tun://:8421/SERVER_IP:8421?net=192.168.123.2/24
```

When no error occurred, you can use the `ip addr` command to inspect the created TUN device:

```
$ ip addr show tun0
2: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1350 qdisc pfifo_fast state UNKNOWN group default qlen 500
    link/none 
    inet 192.168.123.2/24 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::d521:ad59:87d0:53e4/64 scope link flags 800 
       valid_lft forever preferred_lft forever
```

Now you can `ping` the server address:

```
$ ping 192.168.123.1
64 bytes from 192.168.123.1: icmp_seq=1 ttl=64 time=9.12 ms
64 bytes from 192.168.123.1: icmp_seq=2 ttl=64 time=10.3 ms
64 bytes from 192.168.123.1: icmp_seq=3 ttl=64 time=7.18 ms
```

#### iperf3 testing

##### Server side

```
$ iperf3 -s
```

##### Client side

```
$ iperf3 -c 192.168.123.1
```

#### IP routing and firewall rules

If you want the client to access the server network, you need to set the corresponding routing table and firewall rules according to your needs. For example, all the client external network traffic can be forwarded to the server.

##### Server side

Enable IP forwarding and set up firewall rules

```
$ sysctl -w net.ipv4.ip_forward=1

$ iptables -t nat -A POSTROUTING -s 192.168.123.0/24 ! -o tun0 -j MASQUERADE
$ iptables -A FORWARD -i tun0 ! -o tun0 -j ACCEPT
$ iptables -A FORWARD -o tun0 -j ACCEPT
```

##### Client side

Set up firewall rules

{{< hint danger >}}
The following operations will change the client's network environment, unless you know what you are doing, please be careful!
{{< /hint >}}

```
$ ip route add SERVER_IP/32 via eth0   # replace the SERVER_IP and eth0
$ ip route del default   # delete the default route
$ ip route add default via 192.168.123.2  # add new default route
```

## TAP

{{< hint warning >}}
TAP devices are not supported by macOS.
{{< /hint >}} 

### Usage

```
gost -L="tap://[method:password@][local_ip]:port[/remote_ip:port]?net=192.168.123.2/24&name=tap0&mtu=1350&route=10.100.0.0/16&gw=192.168.123.1"
```

## TUN/TAP tunnel over TCP

The TUN/TAP tunnel in GOST is based on the UDP protocol by default.

If you want to use TCP, you can choose the following methods:

### Fake TCP

{{< hint warning >}} 
Fake TCP is not standard TCP, it just simulates the TCP protocol.

This feature is only available on Linux.
{{< /hint >}}

GOST uses [xtaci/tcpraw](https://github.com/xtaci/tcpraw) with built-in support for TCP. This feature can be enabled via the `tcp` option.

##### Server side

```
gost -L "tun://:8421?net=192.168.123.1/24&tcp=true"
```

##### Client side

```
gost -L "tun://:0/SERVER_IP:8421?net=192.168.123.2/24&tcp=true"
```

### Proxy chain (2.9.1+)

You can add a proxy chain to forward UDP data, analogous to UDP port forwarding.

High flexibility, recommended.

{{< hint warning >}} 
The protocol type of the last node of the proxy chain (the last -F parameter) must be GOST SOCKS5, the transport can be any one.
{{< /hint >}}

##### Server side

```
gost -L tun://:8421?net=192.168.123.1/24" -L socks5://:1080
```

##### Client side

```
gost -L tun://:0/:8421?net=192.168.123.2/24 -F socks5://SERVER_IP:1080
```

### GOST port forwarding

Based on UDP port forwarding and proxy chain.

##### Server side

```
gost -L tun://:8421?net=192.168.123.1/24 -L socks5://:1080
```

##### Client side

```
gost -L tun://:8421/:8420?net=192.168.123.2/24 -L udp://:8420/:8421 -F socks5://server_ip:1080
```

### Third-party tools

[udp2raw-tunnel](https://github.com/wangyu-/udp2raw-tunnel).