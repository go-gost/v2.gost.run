+++
date = "2017-11-20T10:51:00+08:00"
title = "HTTP(S)"
url = "http"
weight = 20
+++

HTTP is a protocol type supported by GOST, with a variety of transport types can be very flexible to build proxy services:

## Standard HTTP Proxy Service

```
gost -L http://:8080
```

This is one of the most basic and most commonly used type of proxy service and does not itself have any encryption mechanism.

## Standard HTTPS Proxy Service

```
gost -L https://:443
```

HTTP proxy service using TLS encryption.

## HTTP Over Websocket (Secure)

```
gost -L http+ws://:8080
```

or

```
gost -L http+wss://:443
```

## HTTP Over KCP

```
gost -L http+kcp://:8388
```

KCP is used as the transport type.
