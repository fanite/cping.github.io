---
title: Fail2Ban 的简单应用
date: 2019-09-09 13:54:24
categories: "Linux"
tags: ["Linux","VPS", "CentOS7"]
---

# 这是关于Fail2Ban+Firewalld的应用

> 1、安装fail2ban

```bash
#CentOS内置源并未包含fail2ban，需要先安装epel源
yum -y install epel-release
#安装fial2ban
yum -y install fail2ban
```
> 2、添加在/etc/fail2ban/jail.d/my-jail.local来覆盖默认规则

```ini
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1
bantime = 24h
findtime = 10m
maxretry = 3
# 邮件提醒配置
mta = sendmail
destemail = ****@qq.com
sender = ****@qq.com
# action保存在action.d中
banaction = firewallcmd-multiport
banaction_allports = firewallcmd-allports
```
> 3、邮件配置

```bash
set from=fanite@fanite.com
set smtp=smtps://smtp.exmail.qq.com:465
set smtp-auth-user=xxxxx@qq.com
set smtp-auth-password=4iEsadsadgfdghfci6x7UU
set smtp-auth=login
set smtp-user-starttls
set ssl-verify=ignore
set nss-config-dir=/etc/pki/nssdb/
```
测试: 
```bash
echo "Email Content"|mail -v -s "Test Subject" receiver@qq.com
```

> 4、添加sshd监狱。

```bash
vi /etc/fail2ban/jail.d/sshd.local
```

```ini
[sshd]
enabled = true
filter = sshd
logpath  = /var/log/secure
maxretry = 3
# 默认的action是%(action_)s,如果想要发邮件就必须调用能发邮件的action
action = %(action_mwl)s
```
