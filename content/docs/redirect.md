+++
date = "2017-11-17T15:40:24+08:00"
menu = "main"
title = "透明代理"
weight = 40
+++

GOST在2.3版本中增加了对TCP透明代理的支持。

```bash
gost -L redirect://:12345 -F 192.168.1.1:1080
```

再配合iptables可以实现全局代理。

```bash
iptables -t nat -A OUTPUT -p tcp --match multiport ! --dports 12345,1080 -j DNAT --to-destination 127.0.0.1:12345
```

{{< hint warning >}}
透明代理仅支持Linux系统。
{{< /hint >}}
