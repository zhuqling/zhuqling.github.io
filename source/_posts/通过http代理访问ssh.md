---
title: 通过http代理访问ssh
date: 2016-02-18 09:39:56
tags:
- ssh
- http proxy
- nc
- corkscrew
---
淘宝TAE服务器需要通过http代理才能直接ssh访问实例，首先尝试了使用了nc
```bash
ssh -p PORT USER@IP -o "ProxyCommand=nc -X connect -x sshproxy.tae.aliyun.com:3128 %h %p"
```
- -X 指定协议，connect代表HTTP

但是nc命令不支持有账号密码的http代理。所以改用corkscrew。
在osx下可以直接`brew install corkscrew`

为了不用每次都输入那么长的命令，可以将配置信息写入ssh config文件。

.ssh/config
```bash
Host IP
  Port PORT
  ProxyCommand corkscrew sshproxy.tae.aliyun.com 3128 %h %p ~/.ssh/proxyauth
  ServerAliveInterval   10
```

.ssh/proxyauth
```bash
PROXY_USER:PROXY_PASSWORD
```

以后只需要和平时ssh一样执行`ssh USER@IP`即可。

题外话：淘宝TAE有两个免费的实例配额，建议创建一个nginx实例，可以有子域名支持外网访问，并且是root权限。
