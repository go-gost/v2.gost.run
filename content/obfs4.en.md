+++
date = "2017-11-20T12:57:39+08:00"
menu = "main"
title = "Obfs4"
weight = 36
+++

Obfs4 is a transport type supported by GOST.

First to run the server, generate a URL for client access:

```bash
gost -L ss+obfs4://:18080
```

When it starts up normally, it will display the URL on the console

```bash
ss+obfs4://:18080/?cert=06ss%2FlcDWVkTZLXLcRkH8tozyP0aUXmOm%2BuT5KtbkEP%2BTnCqNumFx9p218Vy0WityAM0Kg&iat-mode=0
```

Then use the generated parameters to start the client:

```bash
gost -L :8080 -F ss+obfs4://server_ip:18080/?cert=06ss%2FlcDWVkTZLXLcRkH8tozyP0aUXmOm%2BuT5KtbkEP%2BTnCqNumFx9p218Vy0WityAM0Kg&iat-mode=0
```

The URL generated here does not contain host IP address, so if the client and the server are not the same host, it is necessary to specify the server's IP.