---
title: 无法删除“.user.ini”文件解决方法
date: 2018-09-10 10:11:46
tags:
  - lnmp
categories:
  - Linux
---

LNMP无法删除或更改权限，显示：`rm: cannot remove '.user.ini': Operation not permitted `
原因:文件被锁定.

解除锁定:
```
chattr -i .user.ini
```

锁定:
```
chattr +i .user.ini
```

文件解锁后就可以删除或编辑了
