+++
date = "2017-11-20T13:24:00+08:00"
menu = "main"
title = "Port Forwarding"
weight = 50
+++

GOST has added support for port forwarding since version 2.1.

## Configuration

Port forwarding service node configuration and ordinary proxy nodes are different:

```bash
scheme://[bind_address]:port/[host]:hostport[,[host]:hostport]?ip=[host]:hostport][,[host]:hostport]]
```

scheme - Forward mode, local: `tcp`, `udp`; remote: `rtcp`, `rudp`; tunnel: `tls`, `kcp`, etc.

[bind_address]:port - Local/remote binding address.

[host]:hostport[,[host]:hostport] - (Optional, 2.6+) Comma-separated target addresses.

**Options**

`ip` - (Optional, 2.8+) Comma-separated target addresses.

`strategy` - (2.6+) Specify node selection strategy, `round` for round-robin, `random` for random selection, `fifo` for top-down selection, the default is `round`.

`max_fails` - (2.8.1+) The maximum number of failed connections for a specified node, When the number of failed connections with a node exceeds this set value, the node will be marked as a **Dead node**, Dead node will not be selected to use. default value is 1.

`fail_timeout` - (2.8.1+) Specify the dead node's timeout period. When a node is marked as a dead node, it will not be selected within this set time interval. After this set time interval, it will participate in node selection again.

## Local TCP port forwarding

Map local TCP port A to the specified destination TCP port B. All data to port A is forwarded to port B. This feature is similar to SSH's local port forwarding feature.

```bash
gost -L=tcp://:2222/192.168.1.1:22 [-F=..]
```

The data of the local TCP port 2222 is forwarded to 192.168.1.1:22 (through the proxy chain if exists).

When the end of the chain (the last -F parameter) is of type `forward+ssh`, GOST will directly use SSH's local port forwarding feature:

```bash
gost -L=tcp://:2222/192.168.1.1:22 -F forward+ssh://:2222
```

Server can be a standard SSH program, it can also be GOST's SSH forwarding mode:

```bash
gost -L forward+ssh://:2222
```

scheme must be `forward+ssh`.

## Remote TCP port forwarding

Map the specified destination TCP port B to local TCP port A. All data to port B is forwarded to port A. This feature is similar to SSH's remote port forwarding feature.

```bash
gost -L=rtcp://:2222/192.168.1.1:22 [-F=... -F=socks5://172.24.10.1:1080]
```

The data of 172.24.10.1:2222 is forwarded to 192.168.1.1:22 (through the proxy chain if exists).

### SOCKS5 BIND method with multiplexing enabled

SOCKS5 BIND method has added support for multiplexing since version 2.5, remote port forwarding can use this feature to improve transport efficiency.

```bash
gost -L rtcp://:8080/192.168.1.1:80 -F socks5://:1080?mbind=true
```

The client enables the SOCKS5 BIND multiplexing mode with the `mbind=true` parameter.

### SSH port forwarding mode

When the end of the chain (the last -F parameter) is of type `forward+ssh`, GOST will directly use SSH's remote port forwarding feature:

```bash
gost -L=rtcp://:2222/192.168.1.1:22 -F forward+ssh://:2222
```

Server can be a standard SSH program, it can also be GOST's SSH forwarding mode:

```bash
gost -L forward+ssh://:2222
```

scheme must be `forward+ssh`.

## Local UDP port forwarding

Map local UDP port A to the specified destination UDP port B. All data to port A is forwarded to port B.

```bash
gost -L=udp://:5353/192.168.1.1:53?ttl=60 [-F=... -F=socks5://172.24.10.1:1080]
```

The data of the local UDP port 5353 is forwarded to 192.168.1.1:53 (through the proxy chain if exists).

Each different client (has different source port) corresponds to a forwarding channel, and each forwarding channel has a timeout. When the timeout is expired and there is no data interaction during this time, the channel will be closed.

The timeout value can be set by the `ttl` parameter. The default value is 60 seconds.

## Remote UDP port forwarding

Map the specified destination UDP port B to local UDP port A. All data to port B is forwarded to port A.

```bash
gost -L=rudp://:5353/192.168.1.1:53?ttl=60 [-F=... -F=socks5://172.24.10.1:1080]
```

The data of 172.24.10.1:5353 is forwarded to 192.168.1.1:53 (through the proxy chain if exists).

Each different client (has different source port) corresponds to a forwarding channel, and each forwarding channel has a timeout. When the timeout is expired and there is no data interaction during this time, the channel will be closed.

The timeout value can be set by the `ttl` parameter. The default value is 60 seconds.

{{< admonition title="NOTE" type="warning" >}}
When forwarding UDP data, if there is a proxy chain, the end of the proxy chain (the last -F parameter) must be a GOST SOCKS5 type proxy, and the GOST will forward UDP data using UDP-over-TCP.
{{< /admonition >}}

## Forward Tunnel

Local TCP port forwarding can be used with transport types in version 2.5:

Server:

```bash
gost -L tls://:443/:1443 -L sni://:1443
```

The `scheme` must be transport type only.

Client:

```bash
gost -L :8080 -F forward+tls://server_ip:443
```

The `scheme` is `forward+transport` format, The protocol type must be `forward`.

### Use Cases

#### Encryption

```bash
gost -L tls://:443/:8080 -L http://:8080
```

Convert the HTTP proxy service on port 8080 to the HTTPS proxy service on port 443.

#### Network Acceleration

```bash
gost -L kcp://:8388/:8338 -L ss://chacha20:123456@:8338
```

Use shadowsocks proxy service on port 8338 with KCP transport on port 8388.