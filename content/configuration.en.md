+++
date = "2017-11-20T10:18:10+08:00"
menu = "main"
title = "Configuration"
weight = 10
+++

{{< admonition title="Logical Layering" type="note" >}}
A proxy service in GOST is logically divided into two layers: Protocol and Transport, There are several optional types on each layer. The two layers are independent of each other and can be used in any combination.
{{< /admonition >}}

When GOST connects to a proxy node, it first interacts with the transport type set by the transport layer. After the transport layer is established, it interacts with the protocol type set by the protocol layer.

## Protocols

* `http` - HTTP

* `http2` - HTTP2

* `socks4` - SOCKS4 (2.4+)

* `socks4a` - SOCKS4A (2.4+)

* `socks5` - SOCKS5

* `ss` - Shadowsocks

* `sni` - SNI (2.5+)

* `forward` - Forward

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

* `ohttp` - HTTP Obfuscation (2.5+)

## Configuration format

Refer to [Port Forwarding](../port-forwarding/) for the node configuration format related to port forwarding。

The configuration of nodes in GOST is similar to the URL format (For `-L` and `-F` parameters):

```bash
[scheme://][user:pass@host]:port[?param1=value1&param2=value2]
```


### **scheme** 

`scheme` can be a separate protocol type or transport type, or a combination of the two, it also can be empty.

#### No type specified

The default transport layer is Raw TCP.

The default protocol layer is HTTP & SOCKS5 for `-L` parameter and HTTP for `-F` parameter.

```bash
gost -L :8080 -F :8888
```

#### Only protocol type is specified

The default transport layer is Raw TCP.

```bash
gost -L http://:8080 -F socks5://:1080
```

#### Only transport type is specified

The default protocol layer is HTTP & SOCKS5 for `-L` parameter and HTTP for `-F` parameter.

```bash
gost -L tls://:443 -F ws://:1443
```

#### Used in combination

```bash
gost -L http+tls://:443 -F socks5+wss://:1443
```

#### Special schemes

In addition to the above types, there are several special shemes:

* `https` - Short form, equivalent to `http+tls`

* `redirect` - TCP transparent proxy (2.3+)

 ```bash
 gost -L redirect://:12345
 ```

* `ssu` - Shadowsocks UDP relay (2.4+)

```bash
gost -L ssu://chacha20:123456@:8338
```

### **Node Authentication**

`user:pass` is used to configure the service's authentication information. For shadowsocks，`user` is the encryption type.

```bash
gost -L admin:123456@:8080 -F ss://chacha20:123456@:8338
```

For HTTP / SOCKS5 services, you can also set multiple groups of authentication information through the `secrets` parameter:

```bash
gost -L=:8080?secrets=secrets.txt
```

The file format of the secrets.txt file is line-by-line authentication information. Each line of authentication information is a user-pass pair separated by space. A line starting with `#` is a comment line.

```text
# period for live reloading
reload      10s

# username password

admin           123456
test\user001    123456
test.user@002   12345678
```

`reload` - This configuration file supports live reloading. This option specifies how often the file is checked for changes, and the live reloading is disabled by default.

{{< admonition title="NOTE" type="warning" >}}
All authentication information is for the protocol layer.
{{< /admonition >}}
