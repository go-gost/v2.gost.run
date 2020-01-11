+++
date = "2017-11-20T12:07:40+08:00"
title = "Shadowsocks"
url = "ss"
weight = 24
+++

Shadowsocks is a protocol type supported by GOST.

Support for shadowsocks is based on library [shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go).

{{< hint warning >}}
**Encryption**

The encryption method and password are mandatory for shadowsocks
{{< /hint >}}

## Usage

### Server side

```
gost -L=ss://chacha20:password@:8338
```

### Client side

```
gost -L=:8080 -F=ss://chacha20:password@server_ip:8338
```

## Shadowsocks UDP relay

{{< hint warning >}}
Currently only the server supports UDP Relay.
{{< /hint >}}

### Server side

```
gost -L=ssu://chacha20:password@:8338
```

## AEAD cipher

As of v2.8, GOST supports AEAD cipher based on [shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2).

### Server side

```
gost -L=ss2://AEAD_CHACHA20_POLY1305:password@:8338
```

### Client side

```
gost -L=:8080 -F=ss2://AEAD_CHACHA20_POLY1305:password@server_ip:8338
```

The shadowsocks protocol can be used in combination with various transport types

## Shadowsocks Over TLS

```
gost -L ss+tls://chacha20:password@:8338
```

## Shadowsocks Over KCP

```
gost -L ss+kcp://chacha20:password@:8338
```
