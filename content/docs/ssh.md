+++
date = "2017-11-17T12:15:24+08:00"
title = "SSH"
url = "ssh"
weight = 34
+++

SSH是GOST支持的一种传输类型(Transport)。

## 使用说明

### 服务端

```
gost -L=ssh://:2222
```

### 客户端

```
gost -L=:8080 -F=ssh://server_ip:2222?ping=60
```

客户端可以通过`ping`参数设置心跳包发送周期，单位为秒。默认不发送心跳包。

## 端口转发

GOST中的SSH也支持标准SSH协议的端口转发功能，具体使用方法请参考[端口转发](../port-forwarding)
