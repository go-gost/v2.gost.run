+++
date = "2017-11-15T16:50:24+08:00"
menu = "main"
title = "Welcome"
type = "homepage"
weight = 0
+++

## GOST - GO Simple Tunnel

### GO语言实现的安全隧道

[![GoDoc](https://godoc.org/github.com/ginuerzh/gost?status.svg)](https://godoc.org/github.com/ginuerzh/gost)
[![Build Status](https://travis-ci.org/ginuerzh/gost.svg?branch=master)](https://travis-ci.org/ginuerzh/gost)
[![Go Report Card](https://goreportcard.com/badge/github.com/ginuerzh/gost)](https://goreportcard.com/report/github.com/ginuerzh/gost)
[![Snap Status](https://build.snapcraft.io/badge/ginuerzh/gost.svg)](https://build.snapcraft.io/user/ginuerzh/gost)

### 特性

* 多端口监听
* 可设置转发代理，支持多级转发(代理链)
* 支持标准HTTP/HTTPS/HTTP2/SOCKS4(A)/SOCKS5代理协议
* 支持多种隧道模式
* SOCKS5代理支持TLS协商加密
* Tunnel UDP over TCP
* 本地/远程TCP/UDP端口转发 (2.1+)
* 支持Shadowsocks协议 (UDP: 2.4+)
* 权限控制 (2.4+)
* 支持SNI代理 (2.5+)
* 负载均衡 (2.5+)

### 下载安装

二进制文件下载: https://github.com/ginuerzh/gost/releases

#### Ubuntu Store

gost已经上架ubuntu store，在ubuntu 16.04上可以直接通过`snap`来安装:

```bash
$ sudo snap install gost
```

### 有问题?

提交Issue: [https://github.com/ginuerzh/gost/issues](https://github.com/ginuerzh/gost/issues)

Google讨论组: [https://groups.google.com/d/forum/go-gost](https://groups.google.com/d/forum/go-gost)

