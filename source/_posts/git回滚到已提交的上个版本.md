---
title: git回滚到已提交的上个版本
date: 2016-02-15 16:52:55
tags:
- git
- git revert
---
当git push后，发现错误提交了代码，可以使用以下命令回滚到上个版本，以你的正确版本号替换下面的版本号
```bash
git reset --hard 56e05fc; git reset --soft HEAD@{1}; git commit
```