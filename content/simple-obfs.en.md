+++
date = "2017-11-17T15:30:24+08:00"
menu = "main"
title = "Simple-obfs"
weight = 35
+++

Simple-obfs is a transport type supported by GOST in version 2.7. It is compatible with [shadowsocks/simple-obfs](https://github.com/shadowsocks/simple-obfs) and Android [Simple Obfuscation](https://play.google.com/store/apps/details?id=com.github.shadowsocks.plugin.obfs_local) plugin. Currently only HTTP mode is supported.

Server:

```bash
gost -L=ss+ohttp://chacha20:123456@:8338
```

Client:

```bash
gost -L=:8080 -F=ss+ohttp://chacha20:123456@server_ip:8338?host=bing.com
```

The client can customize the request host through the `host` parameter.