---
title: tmux入门
date: 2016-08-31 17:09
tags:
- tmux
- iterm
---

使用ssh登陆远程机，每次都要输入ssh user@111.111.111.111，没有配置好ssh免密的话，还要输入密码，如果频繁操作会很麻烦。

用tmux可以解决这个问题，tmux可以持久化连接，只要一个命令又会立即还原你退出时的状态，实在太棒了。

## 新建会话

`tmux new -s session-name`

## 会话列表

`tmux ls`

## 按会话名称连接

`tmux a -t session-name`

## 连接到最近会话

`tmux a`

* iterm下tmux菜单项，需要开启 tmux -CC后才可用，通过dashboard可以图形化管理session。