---
title: "2.KVM-双网卡网关配置"
date: 2019-10-12T01:36:27+08:00
lastmod: 2019-10-12T01:36:27+08:00
keywords: ['middleware']
description: ""
tags: ['kvm','Linux','虚拟机','双网卡']
categories: ['middleware']
author: ""
---
# 双网卡网关配置

1.在系统路由表文件`/etc/iproute2/rt_tables`增加配置
```shell
252 e1 
251 e0
```

2.执行以下命令，并在`/etc/rc.d/network`追加此内容
```shell
# 此内容防止服务器重启路由失效
ip route flush table e0
ip route add default via 10.3.3.1 dev eth0 src 10.3.3.25 table e0                   
ip route add 127.0.0.0/8 dev lo table e0
ip rule add from 10.3.3.25 table e0           
ip route flush table e1
ip route add default via 10.2.2.1 dev eth1 src 10.2.2.10 table e1                     
ip route add 127.0.0.0/8 dev lo table e1
ip rule add from 10.2.2.10 table e1 
```

