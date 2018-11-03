+++
date = "2018-5-19T21:15:00+08:00"
menu = "main"
title = "DNS Control"
weight = 90
+++

GOST added support for custom DNS services in version 2.6, and DNS control can be applied to service nodes.

```bash
gost -L=:8080?dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls
```

The server uses the `dns` parameter to specify a list of DNS services (separated by commas). The format of each DNS service is: `ip[:port][/protocol]`, The port defaults to 53 and the protocol optional values are: `udp`, `tcp`, `tls`. The default is `udp`.

You can also use an external file to specify a list of DNS services:

```bash
gost -L=:8080?dns=dns.txt
```

The format of the configuration file is:

```text
# options
timeout     30s
ttl         60s
reload      10s

# ip[:port] [protocol] [hostname]
8.8.8.8
8.8.8.8     tcp
1.1.1.1     udp
1.1.1.1:53  tcp
1.1.1.1:853 tls     cloudflare-dns.com
```

`timeout` - DNS request timeout, default 30 seconds.

`ttl` - DNS cache expiration, the default is 60 seconds. When set to a negative value, no cache is used.

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

The DNS service list is divided into three columns:

The first column is the DNS server address, the format is ip[:port], and the port default is 53.

The second column is the protocol type. The optional values are: `udp`, `tcp`, `tls`, and the default is `udp`.

The third column is the DNS server domain name. It is valid when the protocol type is `tls` and is used for TLS certificate verification.

## Custom domain name resolution

In addition to the ability to customize the DNS service to resolve domain names, you can also manually specify the domain-IP mapping relationship, similar to the `/etc/hosts` file under Linux.

```bash
gost -L=:8080?hosts=hosts.txt
```

The format of the configuration file:

```text
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