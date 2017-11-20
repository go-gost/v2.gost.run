+++
date = "2017-11-20T12:28:52+08:00"
menu = "main"
title = "Websocket"
weight = 31
+++

Websocket is the transport type supported by GOST. There are four types of Websocket in GOST:

## Websocket

```bash
gost -L ws://:8080
```

Unencrypted websocket tunnel.

## Multiplex Websocket

```bash
gost -L mws://:8080
```

Unencrypted websocket tunnel with multiplexing features.

## Websocket Secure

```bash
gost -L wss://:443
```

Websocket tunnel using TLS encryption.

## Multiplex Websocket Secure

```bash
gost -L mwss://:443
```

Websocket tunnel with multiplexing and TLS encryption.
