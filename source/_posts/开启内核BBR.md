---
title: 开启内核BBR
date: 2018-08-16 15:43:26
categories: "Linux"
tags: ["Linux","VPS","BBR"]
---

大名鼎鼎的TCP BBR拥塞控制算法开启方式：

``` bash
# 查看内核。需要内核4.11+，低版本的内核请先升级内核
uname -r
```

> 开始方法

```bash
modprobe tcp_bbr

echo "tcp_bbr" >> /etc/modules-load.d/modules.conf

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

sysctl -p

# 如果结果都有bbr, 则证明你的内核已开启bbr
# 看到有 tcp_bbr 模块即说明bbr已启动
sysctl net.ipv4.tcp_available_congestion_control

sysctl net.ipv4.tcp_congestion_control
```