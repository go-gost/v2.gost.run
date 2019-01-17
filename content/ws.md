+++
date = "2017-11-17T12:00:24+08:00"
menu = "main"
title = "Websocket"
weight = 31
+++

Websocket是GOST支持的传输类型(Transport)。GOST中的Websocket有四种类型：

## Websocket

```bash
gost -L ws://:8080
```

未加密的websocket隧道。

## Multiplex Websocket

```bash
gost -L mws://:8080
```

具有多路复用特性的未加密websocket隧道。

## Websocket Secure

```bash
gost -L wss://:443
```

使用TLS加密的Websocket隧道。

## Multiplex Websocket Secure

```bash
gost -L mwss://:443
```

具有多路复用特性并使用TLS加密的Websocket隧道。

可以通过`path`参数来设置请求URI，默认值为`/ws`：　

```bash
gost -L ws://:8080?path=/ws
```