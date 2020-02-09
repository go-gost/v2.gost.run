+++
date = "2017-11-15T16:50:24+08:00"
type = "docs"
title = "GOST"
weight = 1
+++

# GO Simple Tunnel

## GO语言实现的安全隧道

[![GoDoc](https://godoc.org/github.com/ginuerzh/gost?status.svg)](https://godoc.org/github.com/ginuerzh/gost)
[![Build Status](https://travis-ci.org/ginuerzh/gost.svg?branch=master)](https://travis-ci.org/ginuerzh/gost)
[![Go Report Card](https://goreportcard.com/badge/github.com/ginuerzh/gost)](https://goreportcard.com/report/github.com/ginuerzh/gost)
[![codecov](https://codecov.io/gh/ginuerzh/gost/branch/master/graphs/badge.svg)](https://codecov.io/gh/ginuerzh/gost/branch/master)
[![GitHub release](https://img.shields.io/github/release/ginuerzh/gost.svg)](https://github.com/ginuerzh/gost/releases/latest)
[![Docker Build Status](https://img.shields.io/docker/build/ginuerzh/gost.svg)](https://hub.docker.com/r/ginuerzh/gost/)
[![Snap Status](https://build.snapcraft.io/badge/ginuerzh/gost.svg)](https://build.snapcraft.io/user/ginuerzh/gost)

[English Version](en/)

## 功能特性

* 多端口监听
* 可设置转发代理，支持多级转发(代理链)
* 支持标准HTTP/HTTPS/HTTP2/SOCKS4(A)/SOCKS5代理协议
* Web代理支持探测防御
* SOCKS5代理支持TLS协商加密
* 支持多种隧道类型
* Tunnel UDP over TCP
* 本地/远程TCP/UDP端口转发
* TCP/UDP透明代理
* 支持Shadowsocks(TCP/UDP)协议
* 支持SNI代理
* 权限控制
* 负载均衡
* 路由控制
* DNS解析和代理
* TUN/TAP设备

## 下载安装

### 二进制文件

[https://github.com/ginuerzh/gost/releases](https://github.com/ginuerzh/gost/releases)

### 源码编译

```
git clone https://github.com/ginuerzh/gost.git
cd gost/cmd/gost
go build
```

### Docker

```
docker pull ginuerzh/gost
```

### Ubuntu Store

```
sudo snap install core
sudo snap install gost
```

## 问题建议

提交Issue: [https://github.com/ginuerzh/gost/issues](https://github.com/ginuerzh/gost/issues)

Telegram讨论群: [https://t.me/gogost](https://t.me/gogost)

Google讨论组: [https://groups.google.com/d/forum/go-gost](https://groups.google.com/d/forum/go-gost)
