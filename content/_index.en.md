+++
date = "2017-11-17T20:34:20+08:00"
type = "docs"
title = "GOST"
weight = 1
+++

# GO Simple Tunnel

## A simple security tunnel written in Golang

[![GoDoc](https://godoc.org/github.com/ginuerzh/gost?status.svg)](https://godoc.org/github.com/ginuerzh/gost)
[![Go Report Card](https://goreportcard.com/badge/github.com/ginuerzh/gost)](https://goreportcard.com/report/github.com/ginuerzh/gost)
[![codecov](https://codecov.io/gh/ginuerzh/gost/branch/master/graphs/badge.svg)](https://codecov.io/gh/ginuerzh/gost/branch/master)
[![GitHub release](https://img.shields.io/github/release/ginuerzh/gost.svg)](https://github.com/ginuerzh/gost/releases/latest)
[![Docker](https://img.shields.io/docker/pulls/ginuerzh/gost.svg)](https://hub.docker.com/r/ginuerzh/gost/)

### ！！！[GOST V3 is available, try it now](https://gost.run)！！！

## Features

* Listening on multiple ports
* Multi-level forward proxies - proxy chain
* Standard HTTP/HTTPS/HTTP2/SOCKS4(A)/SOCKS5 proxy protocols support
* Probing resistance support for web proxy
* TLS encryption via negotiation support for SOCKS5 proxy
* Support multiple tunnel types
* Tunnel UDP over TCP
* Local/remote TCP/UDP port forwarding
* TCP/UDP Transparent proxy
* Shadowsocks Protocol (TCP/UDP)
* SNI Proxy
* Permission control
* Load balancing
* Route control
* DNS resolver and proxy
* TUN/TAP Device


## Installation

### Binary files

[https://github.com/ginuerzh/gost/releases](https://github.com/ginuerzh/gost/releases)

### From Source

```
git clone https://github.com/ginuerzh/gost.git
cd gost/cmd/gost
go build
```

### Docker

```
docker run --rm ginuerzh/gost -V
```

### Ubuntu Store

```
sudo snap install core
sudo snap install gost
```

### Shadowsocks Android Plugin

[xausky/ShadowsocksGostPlugin](https://github.com/xausky/ShadowsocksGostPlugin)


## Any Question

Github Issue: [https://github.com/ginuerzh/gost/issues](https://github.com/ginuerzh/gost/issues)

Telegram Group: [https://t.me/gogost](https://t.me/gogost)

Google Group: [https://groups.google.com/d/forum/go-gost](https://groups.google.com/d/forum/go-gost)


## Donation


Your feedback is the best support!

Buy me a coffee if you want:

**WeChat Reward QR Code**

![WeChat](/img/wechat.png) 

**imToken**

![BTC](/img/btc.png) 
![ETH](/img/eth.png)
![USDT](/img/usdt.png)