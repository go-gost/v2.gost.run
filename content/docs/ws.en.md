+++
date = "2017-11-20T12:28:52+08:00"
title = "Websocket"
url = "ws"
weight = 31
+++

Websocket is the transport type supported by GOST. 

## Usage

```
gost -L "ws://:8080?path=/ws&rbuf=4096&wbuf=4096&compression=false"
```

### Parameters

`path` - request URI，default value is `/ws`　

`rbuf` - receive buffer size in byte，default value is 4096

`wbuf` - send buffer size in byte, default value is 4096

`compression` - compression flag，default value is false


There are four types of Websocket in GOST:

### Standard Websocket

```
gost -L ws://:8080
```

Unencrypted websocket tunnel.

### Multiplex Websocket

```
gost -L mws://:8080
```

Unencrypted websocket tunnel with multiplexing features.

### Websocket Secure

```
gost -L wss://:443
```

Websocket tunnel using TLS encryption.

### Multiplex Websocket Secure

```
gost -L mwss://:443
```

Websocket tunnel with multiplexing and TLS encryption.
