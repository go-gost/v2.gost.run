+++
date = "2017-11-20T10:18:10+08:00"
title = "Configuration"
url = "configuration"
weight = 10
+++

{{< hint info >}}
**Logical Layering**

A proxy service in GOST is logically divided into two layers: Protocol and Transport, There are several optional types on each layer. The two layers are independent of each other and can be used in any combination.
{{< /hint >}}

When GOST connects to a proxy node, it first interacts with the transport type set by the transport layer. After the transport layer is established, it interacts with the protocol type set by the protocol layer.

## Protocols

* `http` - HTTP

* `http2` - HTTP2

* `socks4` - SOCKS4 (2.4+)

* `socks4a` - SOCKS4A (2.4+)

* `socks5` - SOCKS5

* `ss` - Shadowsocks

* `ss2` - Shadowsocks with AEAD support (2.8+)

* `sni` - SNI (2.5+)

* `forward` - Forward

* `relay` - TCP/UDP relay (2.11+)

## Transports

* `tcp` - Raw TCP

* `tls` - TLS

* `mtls` - Multiplex TLS, Add multiplexing on TLS (2.5+)

* `ws` - Websocket

* `mws` - Multiplex Websocket, Add multiplexing on Websocket (2.5+)

* `wss` - Websocket Secure, Websocket based on TLS encryption

* `mwss` - Multiplex Websocket Secure, Add multiplexing on Websocket Secure (2.5+)

* `kcp` - KCP (2.3+)

* `quic` - QUIC (2.4+)

* `ssh` - SSH (2.4+)

* `h2` - HTTP2 (2.4+)

* `h2c` - HTTP2 Cleartext (2.4+)

* `obfs4` - OBFS4 (2.4+)

* `ohttp` - HTTP Obfuscation (2.7+)

* `otls` - TLS Obfuscation (2.11+)

## Configuration format

Refer to [Port Forwarding](/port-forwarding/) for the node configuration format related to port forwarding。

The configuration of nodes in GOST is similar to the URL format (For `-L` and `-F` parameters):

```
[scheme://][user:pass@host]:port[?param1=value1&param2=value2]
```


### **scheme** 

`scheme` can be a separate protocol type or transport type, or a combination of the two, it also can be empty.

#### No type specified

The default transport layer is Raw TCP.

The default protocol layer is HTTP & SOCKS5 for `-L` parameter and HTTP for `-F` parameter.

```
gost -L :8080 -F :8888
```

#### Only protocol type is specified

The default transport layer is Raw TCP.

```
gost -L http://:8080 -F socks5://:1080
```

#### Only transport type is specified

The default protocol layer is HTTP & SOCKS5 for `-L` parameter and HTTP for `-F` parameter.

```
gost -L tls://:443 -F ws://:1443
```

#### Used in combination

```
gost -L http+tls://:443 -F socks5+wss://:1443
```

#### Special schemes

In addition to the above types, there are several special shemes:

* `https` - Short form, equivalent to `http+tls`

* `redirect` - TCP transparent proxy (2.3+)

 ```
 gost -L redirect://:12345
 ```

* `ssu` - Shadowsocks UDP relay (2.4+)

```
gost -L ssu://chacha20:123456@:8338
```

### **Node Authentication**

#### user:pass

`user:pass` is used to configure the service's authentication information. For shadowsocks，`user` is the encryption type.

```
gost -L admin:123456@:8080 -F ss://chacha20:123456@:8338
```

#### auth parameter (2.9.2+)

If the auth info contains special characters, you can use `auth` parameter to encode the info:

```
gost -L :8080?auth=YWRtaW46MTIzNDU2 -F ss://:8338?auth=Y2hhY2hhMjA6QWEjJiEkMTIzNEA1Njc4
```
the value of `auth` is the base64 encoded of `user:pass`.


#### secrets parameter

You can also set multiple groups of authentication information through the `secrets` parameter:

```
gost -L=:8080?secrets=secrets.txt
```

The file format of the secrets.txt file is line-by-line authentication information. Each line of authentication information is a user-pass pair separated by space. A line starting with `#` is a comment line.

```
# period for live reloading
reload      10s

# username password

admin           #123456
test\user001    123456
test.user@002   12345678
```

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

**NOTE:** When the `secrets` parameter is used in the shadowsocks protocol, only the first entry is used as the authentication information.

{{< hint warning >}}
All authentication information is for the protocol layer.
{{< /hint >}}
