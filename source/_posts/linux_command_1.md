---
title: Linux命令复习
date: 2016-09-08 09:50:00
tags:
- linux
- command
- network
---

# 基础命令

ls 排除文件

`ls -I "abc*"`

grep 排除, -v反转，-E扩展匹配

`ls | grep -vE "Filesystem"`

awk 变量及输出

`df -h | awk "{print $1 " " $5}"`

`awk '/MemTotal/{total=$2}/MemFree/{free=$2}END{print (total-free)/1024 " M"}' /proc/meminfo`


df 不自动换行

`df -hP`

# 网络DNS相关

```sh
host -t a www.baidu.com
host -t soa www.baidu.com
host -t cname www.baidu.com
```

```sh
nslookup
> set q=a
> www.baidu.com
> set q=cname
> www.baidu.com
```

```sh
dig @127.0.0.1 -t a www.baidu.com
dig @127.0.0.1 -t soa www.baidu.com
dig @127.0.0.1 -t cname www.baidu.com
```
