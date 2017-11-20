+++
date = "2017-11-20T13:03:15+08:00"
menu = "main"
title = "Transparent Proxy"
weight = 40
+++

GOST added support for TCP Transparent Proxy in version 2.3.

```bash
gost -L redirect://:12345
```

Together with iptables, global proxy can be achieved.

{{< admonition title="NOTE" type="warning" >}}
Transparent proxy supports only Linux system.
{{< /admonition >}}
