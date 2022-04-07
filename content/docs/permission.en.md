+++
date = "2017-11-20T13:57:41+08:00"
title = "Permission Control"
url = "permission"
weight = 60
+++

One can pass available permissions with `whitelist` and `blacklist` values when starting a socks and ssh server. The format for each rule is as follows: `[actions]:[hosts]:[ports]`.

`[actions]` are comma-separted list of allowed actions: `rtcp`, `rudp`, `tcp`, `udp`. can be `*` to encompass all actions.

`[hosts]` are comma-separated list of allowed hosts that one can bind on (in case of `rtcp` and `rudp`), or forward to (incase of `tcp` and `udp`). hosts support globs, like `*.google.com`. can be `*` to encompass all hosts.

`[ports]` are comma-separated list of ports that one can bind to (in case of `rtcp` and `rudp`), or forward to (incase of `tcp` and `udp`), can be `*` to encompass all ports.

Multiple permissions can be passed if seperated with `+`: 

`rtcp,rudp:localhost,127.0.0.1:2222,8000-9000+udp:8.8.8.8,8.8.4.4:53` (allow for reverse tcp and udp binding on localhost and 127.0.0.1 on ports 2222 and 8000-9000 port range, plus allow for udp forwarding to 8.8.8.8 and 8.8.4.4 on port 53)

SSH remote port forwarding can only bind on 127.0.0.1:8000

```
gost -L=forward+ssh://localhost:8389?whitelist=rtcp:127.0.0.1:8000
```

SOCKS5 TCP/UDP remote port forwarding can only bind on ports greater than 1000

```
gost -L=socks://localhost:8389?blacklist=rtcp,rudp:*:0-1000
```

SOCKS5 UDP forwading can only forward to 8.8.8.8:53

```
gost -L=socks://localhost:8389?whitelist=udp:8.8.8.8:53
```
