+++
date = "2017-11-17T15:30:24+08:00"
menu = "main"
title = "Obfs4"
weight = 36
+++

Obfs4是GOST支持的一种传输类型(Transport)。

首先要运行服务端，生成客户端访问的URL：

```bash
gost -L ss+obfs4://:18080
```

当正常启动后，会在控制台显示URL

```bash
ss+obfs4://:18080/?cert=06ss%2FlcDWVkTZLXLcRkH8tozyP0aUXmOm%2BuT5KtbkEP%2BTnCqNumFx9p218Vy0WityAM0Kg&iat-mode=0
```

然后再使用生成的参数启动客户端：

```bash
gost -L :8080 -F ss+obfs4://server_ip:18080/?cert=06ss%2FlcDWVkTZLXLcRkH8tozyP0aUXmOm%2BuT5KtbkEP%2BTnCqNumFx9p218Vy0WityAM0Kg&iat-mode=0
```

这里生成的地址不包含IP，所以如果客户端与服务端不在一起，就要指定服务端的IP。