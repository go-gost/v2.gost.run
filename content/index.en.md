+++
date = "2017-11-17T20:34:20+08:00"
menu = "main"
type = "homepage"
title = "GOST"
weight = 1
+++

# GO Simple Tunnel

## A simple security tunnel written in Golang

[![GoDoc](https://godoc.org/github.com/ginuerzh/gost?status.svg)](https://godoc.org/github.com/ginuerzh/gost)
[![Build Status](https://travis-ci.org/ginuerzh/gost.svg?branch=master)](https://travis-ci.org/ginuerzh/gost)
[![Go Report Card](https://goreportcard.com/badge/github.com/ginuerzh/gost)](https://goreportcard.com/report/github.com/ginuerzh/gost)
[![GitHub release](https://img.shields.io/github/release/ginuerzh/gost.svg)](https://github.com/ginuerzh/gost/releases/latest)
[![Docker Build Status](https://img.shields.io/docker/build/ginuerzh/gost.svg)](https://hub.docker.com/r/ginuerzh/gost/)
[![Snap Status](https://build.snapcraft.io/badge/ginuerzh/gost.svg)](https://build.snapcraft.io/user/ginuerzh/gost)

## Features

* Listening on multiple ports
* Multi-level forward proxies - proxy chain
* Standard HTTP/HTTPS/HTTP2/SOCKS4(A)/SOCKS5 proxy protocols support
* TLS encryption via negotiation support for SOCKS5 proxy
* Support multiple tunnel types
* Tunnel UDP over TCP
* Local/remote TCP/UDP port forwarding
* Transparent TCP proxy
* Shadowsocks Protocol (TCP/UDP)
* SNI Proxy
* Permission control
* Load balancing

## Download

Binary file download: [https://github.com/ginuerzh/gost/releases](https://github.com/ginuerzh/gost/releases)

### Docker

```bash
docker pull ginuerzh/gost
```

### Ubuntu Store

GOST has been released in ubuntu store, and can be installed directly through the snap in ubuntu 16.04:

```bash
sudo snap install gost
```

## Any Question

Github Issue: [https://github.com/ginuerzh/gost/issues](https://github.com/ginuerzh/gost/issues)

Google Group: [https://groups.google.com/d/forum/go-gost](https://groups.google.com/d/forum/go-gost)
