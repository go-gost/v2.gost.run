+++
date = "2018-5-19T20:12:00+08:00"
title = "路由控制"
url = "bypass"
weight = 80
+++

GOST在2.6版本中增加了路由控制功能，可以通过黑白名单来控制客户端的请求。路由控制可以应用于服务节点(`-L`参数)和代理链的所有层级节点(`-F`参数)。

```bash
gost -L=:8080?bypass=127.0.0.1,192.168.1.0/24,.example.net -F=:1080?bypass=172.10.0.0/16,localhost,*.example.com
```

通过`bypass`参数来指定请求的目标地址列表(以逗号分割的IP,CIDR,域名或域名通配符地址)。

当执行代理链的节点选择时，每当确定一个代理链层级节点后，会应用此节点上的路由配置(`bypass`参数)，若此次请求的目标地址包含在此配置中，则代理链终止于此节点(且不包含此节点)。

若地址比较多，可以使用外部配置文件：

```bash
gost -L :8080?bypass=bypass.txt -F :1080?bypass=bypass2.txt
```

配置文件的格式为(地址列表和可选的配置项)：

```text
# options
reload   10s
reverse  true

# bypass addresses
127.0.0.1
172.10.0.0/16
localhost
*.example.com
.example.org
```

`reload` - 此配置文件支持热更新。此选项用来指定文件检查周期，默认关闭热更新。

`reverse` - 指定是否切换为白名单。

若要转换为白名单，则通过在`bypass`参数值前添加`~`前缀：

```bash
gost -L=:8080?bypass=~127.0.0.1,172.10.0.0/16,localhost,*.example.com,.example.org
```

对于文件方式也是一样：

```bash
gost -L=:8080?bypass=~bypass.txt
```
