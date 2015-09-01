title: How to debug the freeBSD kernel
toc: true
date: 2015-09-01 14:46:42
tags: [freeBSD]
categories: Linux
description: This is a work note when I work in YuanV.
feature: http://7xj4cp.com1.z0.glb.clouddn.com/freebsd.png
---

### 内核调试

    kgdb kernel.debug vmcore

在负载上面，kernel.debug文件名字为/kernel/SECkernel

使用bt查看栈

使用f [num]查看对应id号的栈信息