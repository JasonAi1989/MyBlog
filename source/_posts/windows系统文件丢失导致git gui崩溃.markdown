title: windows系统文件丢失导致git gui崩溃 
toc: true
date: 2015-05-15 12:46
tags: [git]
categories: git
description:
feature:
---

当我重新安装Python时出现的这个问题，但目测和Python没有直接的关系。

先上一张git Gui崩溃时的图:

![git Gui crash](http://7xj4cp.com1.z0.glb.clouddn.com/gitGuiCrash.jpg)

<!-- more -->

在网上搜索一番后得到如下的一份结果：

    My name is hyungon.kim (Korea)
    I got the same error message.
    -Couldn’t reserve space for cygwin’s heap, Win32 error
    487
    I trying to several times to solve this problem.
    I searching and searching Internet many times.
    but I couldn’t find a solution.

    In 5~6 hours, finally I found it a solution.
    solution————————————–
    first, You must have rebase.exe
    If you search Internet, you find easily, and downloading.
    second, rebase -b 0x76000000 /winavr/utils/bin/msys-1.0.dll
    0x76000000(examlple) -> you can change this address value
    properly, maybe Winavr is compiled well.

然后我就按照这个解决方案执行了一下，先到网上下载rebase.exe，在执行如下命令：

```
rebase -b 0x68570000 C:\Program Files (x86)\Git\bin\msys-1.0.dll
```
命令顺利执行，然后再次打开git gui，崩溃消失