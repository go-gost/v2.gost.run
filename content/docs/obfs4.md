+++
date = "2017-11-17T15:30:24+08:00"
title = "Obfs4"
url = "obfs4"
weight = 36
+++

Obfs4是GOST支持的一种传输类型(Transport)。

首先要运行服务端，生成客户端访问的URL：

```
gost -L ss+obfs4://:18080
```

当正常启动后，会在控制台显示URL

```
ss+obfs4://:18080/?cert=06ss%2FlcDWVkTZLXLcRkH8tozyP0aUXmOm%2BuT5KtbkEP%2BTnCqNumFx9p218Vy0WityAM0Kg&iat-mode=0
```

然后再使用生成的参数启动客户端：

```
gost -L :8080 -F ss+obfs4://server_ip:18080/?cert=06ss%2FlcDWVkTZLXLcRkH8tozyP0aUXmOm%2BuT5KtbkEP%2BTnCqNumFx9p218Vy0WityAM0Kg&iat-mode=0
```

这里生成的URL不包含IP，所以如果客户端与服务端不是同一台主机，就要指定服务端的IP。
