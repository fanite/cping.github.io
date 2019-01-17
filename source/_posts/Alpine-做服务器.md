---
title: Alpine 做服务器
date: 2019-01-17 19:12:57
tags:
---

## 打开sshd
```bash
vi /etc/ssh/sshd_config
```

设置`PermitRootLogin`为`yes`

---

## 设置网络
查看官网[WIKI](https://wiki.alpinelinux.org/wiki/Configure_Networking)

---

## 打开BBR
```bash
cd /etc/sysctl.d/
vi 01-bbr.conf
echo "net.core.default_qdisc=fq" >> 01-bbr.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> 01-bbr.conf
```

