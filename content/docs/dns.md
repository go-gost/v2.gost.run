+++
date = "2018-5-19T21:15:00+08:00"
title = "DNS代理"
url = "dns"
weight = 95
+++

GOST在2.10版本中增加了DNS代理服务。

## 使用说明

```
gost -L="dns://:1053?mode=udp&dns=8.8.8.8,1.1.1.1:53/tcp,1.1.1.1:853/tls,https://1.0.0.1/dns-query"
```

### 参数项

`mode` - 指定代理服务的运行模式，可选值：`udp`，`tcp`，`tls`，`https`。默认值为`udp`。

`dns` - 与[DNS解析](/resolver/)中的`dns`参数功能和使用方式相同。

`ttl` - 2.10.1+，缓存过期时间，设置负值则禁用缓存，默认使用查询结果中的过期时间。

`timeout` - 2.10.1+，请求超时时间，默认5s。

`ip` - 2.10.1+，客户端IP，设置后会开启ECS(EDNS Client Subnet)扩展功能。