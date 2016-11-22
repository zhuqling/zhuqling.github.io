---
title: git 重设远程源
date: 2016-02-15 17:01:33
tags:
- git
- git rebase
---
当git远程源有变更或设置错误了源时，使用如下命令个性远程源

```bash
git remote rm origin;
git remote add git@192.168.2.4:root/dd-push.git;
git push --set-upstream origin master;
```