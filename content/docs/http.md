+++
date = "2017-11-17T10:50:24+08:00"
title = "HTTP(S)"
url = "http"
weight = 20
+++

HTTP是GOST支持的一种协议类型(Protocol)，配合各种传输类型(Transport)可以很灵活的构建代理服务：

## 标准HTTP代理服务

```bash
gost -L http://:8080
```

这是一种最基本的也是最普遍使用的代理服务类型，本身没有任何加密机制。

## 标准HTTPS代理服务

```bash
gost -L https://:443
```

使用了TLS加密的HTTP代理服务。

## HTTP Over Websocket

```bash
gost -L http+ws://:8080
```

或

```bash
gost -L http+wss://:443
```

使用Websocket (secure)作为传输类型。

## HTTP Over KCP

```bash
gost -L http+kcp://:8388
```

使用KCP作为传输类型。
