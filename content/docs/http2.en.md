+++
date = "2017-11-20T12:49:31+08:00"
menu = "main"
title = "HTTP2"
weight = 21
+++

In GOST HTTP2 has two modes: proxy mode and tunnel mode.

## Proxy Mode

In proxy mode, HTTP2 is used as the protocol type and the transport type must be empty.

```bash
gost -L http2://:443
```

As a standard HTTP2 proxy service.

{{< hint info >}}
Proxy mode only supports the use of TLS encryption HTTP2 protocol, does not support plaintext HTTP2.
{{< /hint >}}

## Tunnel Mode

In tunnel mode, HTTP2 is used as the transport type: `h2` and `h2c`.

```bash
gost -L socks5+h2://:443
```

or

```bash
gost -L http+h2c://:443
```

Tunnel mode supports encryption (h2) and plaintext (h2c) two modes.