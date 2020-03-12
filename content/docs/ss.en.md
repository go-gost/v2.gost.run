+++
date = "2017-11-20T12:07:40+08:00"
title = "Shadowsocks"
url = "ss"
weight = 24
+++

Shadowsocks is a protocol type supported by GOST.

Support for shadowsocks is based on library [shadowsocks/shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go).

## TCP

{{< hint warning >}}
Starting from 2.10.1+, encryption becomes optional.
{{< /hint >}}

##### Server side

```
gost -L=ss://chacha20:password@:8338
```

##### Client side

```
gost -L=:8080 -F=ss://chacha20:password@server_ip:8338
```

## AEAD cipher

{{< hint warning >}}
As of 2.10.1+, features of `ss2` have been merged into `ss`, the AEAD cipher methods can be used directly in `ss`, and ` ss2` is deprecated.
{{< /hint >}}

As of v2.8, GOST supports AEAD cipher based on [shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2).

##### Server side

```
gost -L=ss2://AEAD_CHACHA20_POLY1305:password@:8338
```

##### Client side

```
gost -L=:8080 -F=ss2://AEAD_CHACHA20_POLY1305:password@server_ip:8338
```

### Combined with transport layer

The shadowsocks protocol can be used in combination with various transport types

#### Shadowsocks Over TLS

```
gost -L ss+tls://chacha20:password@:8338
```

#### Shadowsocks Over KCP

```
gost -L ss+kcp://chacha20:password@:8338
```

## UDP

##### Server side

```
gost -L=ssu://chacha20:password@:8338
```

`ttl` - (2.10+) tunnel time to live, default values is 60s.

### Changes after 2.10+

#### Client support

`ssu` can be used in the chain to forward UDP data:

```
gost -L udp://:5353/8.8.8.8:53 -F=ssu://method:password@:8338
```

#### Encryption

Encryption becomes optional.

GOST supports AEAD cipher methods based on [shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2), and compatible with cipher methods in older version. 

When encryption is enabled, the cipher methods in (shadowsocks/shadowsocks-go) will be used first. If not supported, switch to the (shadowsocks/go-shadowsocks2).

#### Combined with transport layer

`ssu` became a protocol type, so it can be used in combination with transport types.

```
gost -L ssu+kcp://:8338
```

```
gost -L ssu+wss://:443
```

By default, `ssu` is equivalent to` ssu + udp`, and the transport layer is the raw UDP protocol. In this case, GOST will use shadowsocks UDP relay protocol for data forwarding.

If other transport types are specified, SOCKS5 UDP relay protocol is used for data forwarding.

{{< hint warning >}}
`ssu` can only be used to forward UDP data. The behavior of forwarding TCP data is undefined.

When used in a chain, ssu must be the last node of the chain.

When `ssu+udp` is used in a chain, no other nodes should exist in the chain.
{{< /hint >}}