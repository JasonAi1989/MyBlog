title: freeBSD内核参数的获取、设置与添加
toc: true
date: 2015-07-23 14:46:42
tags: [freeBSD]
categories: freeBSD 
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/freebsd.png
---

在freeBSD的内核中有很多系统参数，比如关于网络的、关于系统参数的等等，这些参数有些我们可以直接在命令行和用户态程序中进行获取和设置，方法如下：

### 在命令行中获取和设置

通过命令sysctl来获取和设置，可以通过

    sysctl -a
    
来查看系统中所有的内核参数

<!--more-->

获取单个参数就在sysctl后面跟上这个内核参数的键，例如：

    systcl net.inet.eccomtcp.jpeg_quality
    
设置单个参数就在键的后面跟上对应要设置的值，例如：

    systcl net.inet.eccomtcp.jpeg_quality=12
    
---
### 在用户态中获取和设置

在用户态程序中获取和设置，可以通过函数sysctlbyname来完成，

    int	sysctlbyname(const char *, void *, size_t *, void *, size_t);

    arg1[char*]  要获取或者修改的内核键值，如net.inet.eccomtcp.alert_buffer
    arg2[void*]  用于获取内核参数，内核参数的结构buf
    arg3[size_t *]   用于获取内核参数，内核参数结构的长度
    arg4[void*]  用于修改内核参数，内核参数的结构buf
    arg5[size_t]  用于修改内核参数，内核参数结构的长度
    返回值：调用失败时返回-1
    
---

### 在内核中增加一个内核参数

随便在某个内核源文件中添加即可。

    uint32_t jpeg_quality = 50;
    SYSCTL_INT(_net_inet_eccomtcp, OID_AUTO, jpeg_quality,  CTLFLAG_RW, &jpeg_quality, 0, "");


具体使用哪个函数来注册自定义的内核参数请参考man SYSCTL_INT，在此只简单介绍一下参数类型与所对应的注册函数。

    SYSCTL_INT       整型，无符号、有符号均可
    SYSCTL_STRING    字符串型
    SYSCTL_STRUCT    结构体
    
再简单介绍一下顶层的sysctl用户空间

    compat      兼容性方面的信息

    debug       debug信息，其下很多的子项用于具体的调试工作

    hw          硬件和驱动信息

    kern        内核

    machdep     Machine-dependent 设备依赖配置

    net         网络

    regression  Regression test configuration and information.不知道是干什么的

    security    安全

    sysctl      为sysctl预留的名字空间

    user        为用户配置设置的名字空间

    vfs         虚拟文件系统配置和信息

    vm          虚拟内存子系统配置和信息
