+++
date = "2017-11-20T13:03:15+08:00"
title = "Transparent Proxy"
url = "redirect"
weight = 40
+++

GOST added support for TCP Transparent Proxy in version 2.3.

{{< hint warning >}}
Transparent proxy is only available on Linux.
{{< /hint >}}

## TCP

```
gost -L redirect://:12345 -F 192.168.1.1:1080
```

### Local global TCP proxy

#### iptables rules

```
iptables -t nat -A OUTPUT -p tcp --match multiport ! --dports 12345,1080 -j DNAT --to-destination 127.0.0.1:12345
```

## UDP (2.10+)

UDP transparent proxy is based on iptables tproxy module.

```
gost -L redu://:12345?ttl=60s -F ssu://1.2.3.4:1080
```

`ttl` - per tunnel time to live, default value is 60s.

### Local global UDP proxy

#### iptables rules

{{< hint warning >}}
`192.168.0.0/16` in the rules is the network where the machine is located, and `1.2.3.4/32` is the forwarding server address. Please modify them according to your environment.
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

#### routing table

```
ip rule add fwmark 1 table 100
ip route add local 0.0.0.0/0 dev lo table 100
```