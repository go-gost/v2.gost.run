+++
date = "2017-11-20T12:18:24+08:00"
title = "TLS"
url = "tls"
weight = 30
+++

TLS is a transport type supported by GOST.

Server:

```bash
gost -L tls://:443
```

Client:

```bash
gost -L :8080 -F tls://server_ip:443
```

GOST extends the TLS transport type (mtls) with multiplex features on the basis of TLS.

Server:

```bash
gost -L mtls://:443
```

Client:

```bash
gost -L :8080 -F mtls://server_ip:443
```

## TLS Certificate

There is built-in TLS certificate in gost, if you need to use other TLS certificate, there are two ways:

* Place two files `cert.pem` (public key) and `key.pem` (private key) in the current working directory, gost will automatically load them.

* Use the parameters `key` and `cert` to specify the path to the certificate files:

```bash
gost -L="tls://:443?cert=/path/to/my/cert/file&key=/path/to/my/key/file"
```

## Certificate Verification

Client can specify `secure` parameter to perform server's certificate chain and host name verification (Default does not verify the certificate):

```bash
gost -L=:8080 -F="tls://server_domain_name:443?secure=true"
```

When you need to verify the certificate, the `server_domain_name` section of the node configuration must fill in the server's domain name.

## Certificate Pinning

Client can specify the CA certificate via the `ca` parameter to perform [Certificate Pinning](https://en.wikipedia.org/wiki/Transport_Layer_Security#Certificate_pinning)(Certificate Pinning):

```bash
gost -L=:8080 -F="tls://:443?ca=ca.pem"
```

{{< hint info >}}
The above parameters can be used for all TLS-enabled services,such as HTTP2, QUIC, WSS, SSH, SOCKS5.
{{< /hint >}}
