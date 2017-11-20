+++
date = "2017-11-20T12:40:44+08:00"
menu = "main"
title = "QUIC"
weight = 33
+++

QUIC is a transport type supported by GOST.

Support for QUIC is based on library [lucas-clemente/quic-go](https://github.com/lucas-clemente/quic-go).

Server:

```bash
gost -L=quic://:6121
```

Client:

```bash
gost -L=:8080 -F=quic://server_ip:6121
```

## Keep Alive

Client can use the `keepalive` parameter to start heartbeat detection

```bash
gost -L=:8080 -F=quic://server_ip:6121?keepalive=true
```

{{< admonition title="NOTE" type="warning" >}}
To use a QUIC node in a proxy chain, there can be only one QUIC node in the proxy chain, and this node can only serve as the first node in the proxy chain.
{{< /admonition >}}
