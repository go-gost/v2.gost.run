+++
date = "2017-11-20T12:12:40+08:00"
title = "SNI"
url = "sni"
weight = 25
+++

SNI is a protocol type supported by GOST.

## Usage

### Server side

```
gost -L sni://:443
```

### Client side

The SNI proxy service can be used directly by configuring hosts, or use GOST for forwarding:

```
gost -L :8080 -F sni://server_ip:443
```

## Host obfuscation

In GOST, the SNI client can specify the host alias by using the `host` parameter:

```
gost -L :8080 -F sni://server_ip:443?host=example.com
```

The SNI client replaces the host name in the TLS handshake or HTTP request header with the one specified by the `host` parameter.

The SNI protocol can be used in combination with various transport types

### SNI Over TLS

```
gost -L sni+tls://:443
```

### SNI Over Websocket

```
gost -L sni+ws://:443
```
