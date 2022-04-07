+++
date = "2018-5-19T21:15:00+08:00"
title = "DNS Resolver"
url = "resolver"
weight = 90
+++

GOST added support for custom DNS resolver in version 2.6, and can be applied to service nodes. When the node receives a request, it resolves the target address of this request using the DNS service specified on this node.

```
gost -L=:8080?dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls,https://1.0.0.1/dns-query
```

`dns` - specifies a list of DNS services (separated by commas). The format of each DNS service is: `ip[:port][/protocol]`, The port defaults to 53 and the protocol optional values are: `udp`, `udp-chain`, `tcp`, `tcp-chain`, `tls`, `tls-chain`, `https`, `https-chain`. The default is `udp`.

### Proxy Chain (2.10.0+)

If you need to connect to a DNS server through proxy chain, you can use the following protocols:

`udp-chain`, `tcp-chain`, `tls-chain`, `https-chain`

```
gost -L=http://:8080?dns=8.8.8.8/udp-chain,1.1.1.1:853/tls-chain,https-chain://dns.google/dns-query -F=socks://:1080
```

{{< hint warning >}}
If you want to forward `udp-chain`, The protocol type of the last node of the proxy chain (the last -F parameter) must be GOST SOCKS5, the transport can be any one.
{{< /hint >}}

You can also use an external file to specify a list of DNS services:

```
gost -L=:8080?dns=dns.txt
```

The format of the configuration file is:

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
https://1.0.0.1/dns-query    https-chain
```

`timeout` - DNS request timeout, default 5 seconds.

`ttl` - DNS cache expiration, default to the TTL in DNS server response. When set to a negative value, no cache is used.

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

`prefer` - (2.8.2+) AAAA(IPv6) lookups before A(IPv4).

`ip` - 2.10.1+，client host IP. Will enable the ECS(EDNS Client Subnet) extension if set.

The DNS service list is divided into three columns:

The first column is the DNS server address, the format is ip[:port], and the port default is 53.

The second column is the protocol type. The optional values are: `udp`, `tcp`, `tls`, `https`, and the default is `udp`.

The third column is the DNS server domain name. It is valid when the protocol type is `tls` and is used for TLS certificate verification.

## Custom domain name resolution

In addition to the ability to customize the DNS service to resolve domain names, you can also manually specify the domain-IP mapping relationship, similar to the `/etc/hosts` file under Linux.

```
gost -L=:8080?hosts=hosts.txt
```

The format of the configuration file:

```
# options
reload  10s

# IP_address    canonical_hostname     [aliases...]
127.0.0.1       localhost
192.168.1.10    foo.mydomain.org       foo
192.168.1.13    bar.mydomain.org       bar baz
```

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

The mapping entries are divided into three columns:

The first column is the IP address.

The second column is the host name or domain name.

The third column is alias and can have more than one.
