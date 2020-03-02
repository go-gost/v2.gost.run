+++
date = "2017-11-20T12:45:12+08:00"
title = "SSH"
url = "ssh"
weight = 34
+++

SSH is a transport type supported by GOST.

## Usage

##### Server side

```
gost -L=ssh://:2222
```

##### Client side

```
gost -L=:8080 -F=ssh://server_ip:2222?ping=60
```

Client can use the `ping` parameter to set the heartbeat sending period in seconds. By default, no heartbeat packet is sent.

## Port Forwarding

GOST SSH also supports standard SSH protocol port forwarding function, please refer to [Port Forwarding](../port-forwarding) for more detail.

## PubKey authentication (2.11+)

##### Server side

```
gost -L="ssh://:2222?ssh_authorized_keys=/path/to/authorized_keys"
```

`ssh_authorized_keys` - authorized public keys file

##### Client side

```
gost -L :8080 -F=ssh://server_ip:2222?ssh_key=/path/to/id_rsa"
```

`ssh_key` - private key file