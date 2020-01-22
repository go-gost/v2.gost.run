+++
date = "2018-5-19T21:15:00+08:00"
title = "DNS Proxy"
url = "dns"
weight = 95
+++

GOST added support for DNS proxy in version 2.10.

## Usage

```
gost -L="dns://:1053?mode=udp&dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls,https://1.0.0.1/dns-query"
```

### Parameters

`mode` - proxy mode, optional values are: `udp`，`tcp`，`tls`，`https`. Default value is `udp`.

`dns` - same as the `dns` parameter in [DNS Resolver](../resolver/).

