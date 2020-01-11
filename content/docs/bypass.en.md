+++
date = "2018-11-03T14:00:00+08:00"
title = "Routing Control"
url = "bypass"
weight = 80
+++

GOST added routing control in version 2.6 to control client requests through black and white lists. Routing control can be applied to service nodes (`-L` parameters) and all hierarchical nodes of the proxy chain (`-F` parameters).

```
gost -L=:8080?bypass=127.0.0.1,192.168.1.0/24,.example.net -F=:1080?bypass=172.10.0.0/16,localhost,*.example.com
```

Specify the requested destination address list (comma-separated IP, CIDR, domain name or domain name wildcard address) via the `bypass` parameter.

When the node selection of the proxy chain is performed, the routing configuration (`bypass` parameter) on this node is applied every time a proxy chain node is determined. If the target address of the request is included in this configuration, the proxy chain terminates at this node (and do not include this node).

If there are more addresses, you can use an external configuration file:

```
gost -L :8080?bypass=bypass.txt -F :1080?bypass=bypass2.txt
```

The format of the configuration file is (address list and optional configuration options):

```
# options
reload   10s
reverse  true

# bypass addresses
127.0.0.1
172.10.0.0/16
localhost
*.example.com
.example.org
```

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

`reverse` - Specifies whether to switch to a whitelist.

To convert to a whitelist, you can also add the `~` prefix before the `bypass` parameter value:

```
gost -L=:8080?bypass=~127.0.0.1,172.10.0.0/16,localhost,*.example.com,.example.org
```

The same is true for configuration file:

```
gost -L=:8080?bypass=~bypass.txt
```
