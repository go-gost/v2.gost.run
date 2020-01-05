+++
date = "2017-11-17T12:00:24+08:00"
menu = "main"
title = "Websocket"
weight = 31
+++

Websocket是GOST支持的传输类型(Transport)。

## 参数说明

```bash
gost -L "ws://:8080?path=/ws&rbuf=4096&wbuf=4096&compression=false"
```

`path` - 设置请求URI，默认值为`/ws`

`rbuf` - 接收缓冲区大小，默认值：4096

`wbuf` - 发送缓冲区大小，默认值：4096

`compression` - 是否使用压缩，默认值：false

GOST中的Websocket有四种类型：

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