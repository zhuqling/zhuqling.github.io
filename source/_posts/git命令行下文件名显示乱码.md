---
title: Git命令行乱码与放弃本地变更
date: 2017-11-17 13:54
tags:
- Git
---

# git 命令行下文件名显示乱码

```
git config –global core.quotepath false
git config core.quotepath false
```

# 当提示”Your branch is ahead of 'origin/master' by 5 commits“时，需要强制同步到最新，放弃本地变更

```
git pull --rebase
```
