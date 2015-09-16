title: freebsd内核时钟
toc: true
date: 2015-03-06 18:31
tags: [freeBSD] [时钟]
categories: freeBSD
description:
feature: http://7xj4cp.com1.z0.glb.clouddn.com/freebsd.png
---

freebsd中的ticks64相当于linux中的jiffies，记录从电脑开启至当前的所有时钟中断数

ticks64/hz    就是电脑开机至当前所经历的秒数

hz一般是1000或者100

<!-- more -->

hz的含义是一秒内的时钟中断数

关于时钟的相关定义在sys/kern/kern_clock.c文件中

时钟是作为一个独立模块初始化的，初始化代码为

SYSINIT(clocks, SI_SUB_CLOCKS, SI_ORDER_FIRST, initclocks, NULL);

具体的初始化函数为initclocks