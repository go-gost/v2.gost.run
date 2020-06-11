+++
date = "2019-01-05T14:36:00+08:00"
title = "Probing Resistance"
url = "probe_resist"
weight = 22
+++

GOST added probing resistance to the HTTP/HTTPS/HTTP2 proxy in version 2.7. When the proxy server receives an invalid request, it will return the corresponding response according to the probing resistance policy.

{{< hint warning >}}
The probing resistance feature is only valid when the proxy server has user authentication enabled.
{{< /hint >}}

```
gost -L="http2://admin:123456@:443?probe_resist=code:400&knock=www.example.com"
```

## probe_resist

The proxy server specifies the policy through the `probe_resist` parameter. The format of the parameter is：`type:value`.

The optional values for type are:

* `code` - Corresponding value is HTTP response code. The proxy server will reply to client the specified status code. For example: `probe_resist=code:404`

* `web` - Corresponding value is HTTP URL. The proxy server will request this URL using HTTP GET method and return the response to the client. For example: `probe_resist=web:example.com/page.html`

* `host` - Corresponding value is host[:port]. The proxy server forwards the client request to the specified host and returns the host's response to the client. The proxy server is equivalent to the port forwarding service here. For example: `probe_resist=host:example.com:8080`

* `file` - The corresponding value is the local file path. The proxy server will reply to the client 200 response code, and the content of the specified file is sent to the client as response Body. For example: `probe_resist=file:/path/to/file.txt`

```
gost -L=http://admin:123456@:8080?probe_resist=code:403
```

```
gost -L=https://admin:123456@:443?probe_resist=host:www.example.com:8080
```

```
gost -L=http2://admin:123456@:443?probe_resist=file:/send/to/client/file.txt
```

## knock (2.8+)

After the probe resistance is enabled, the server will not respond the `407 Proxy Authentication Required` to the client by default when authentication fails. But in some cases the client needs the server to tell if it needs authentication (for example, the SwitchyOmega plugin in Chrome). Set a private host with the `knock` parameter, the server will only send a `407` response when accessing this host.

This feature can be applied to HTTP, HTTPS and HTTP/2 proxies:

```
gost -L=http://admin:123456@:8080?probe_resist=code:403&knock=www.example.com
```

```
gost -L=https://admin:123456@:443?probe_resist=host:www.example.com:8080&knock=www.example.com
```

```
gost -L=http2://admin:123456@:443?probe_resist=file:/send/to/client/file.txt&knock=www.example.com
```
