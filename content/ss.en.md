+++
date = "2017-11-20T12:07:40+08:00"
menu = "main"
title = "Shadowsocks"
weight = 22
+++

Shadowsocks is a protocol type supported by GOST.

Support for shadowsocks is based on library [shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go).

{{< admonition title="Encryption" type="note" >}}
The encryption method and password are mandatory for shadowsocks
{{< /admonition >}}

Server:

```bash
gost -L=ss://chacha20:123456@:8338
```

Client:

```bash
gost -L=:8080 -F=ss://chacha20:123456@server_ip:8338
```

## Shadowsocks UDP relay

Currently only the server supports UDP Relay.

Server:

```bash
gost -L=ssu://chacha20:123456@:8338
```

The shadowsocks protocol can be used in combination with various transport types

## Shadowsocks Over TLS

```bash
gost -L ss+tls://chacha20:123456@:8338
```

## Shadowsocks Over KCP

```bash
gost -L ss+kcp://chacha20:123456@:8338
```
