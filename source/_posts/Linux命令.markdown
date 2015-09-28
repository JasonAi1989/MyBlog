title: Basic command on Linux
toc: true
date: 2015-06-04 14:46:42
tags: [Linux]
categories: Linux 
description:
---

lsattr 显示文件的特殊属性

chattr 修改文件的特殊属性，比如禁止被删除、修改等

date   显示当前时间

<!--more-->

uptime 在一行上显示如下信息：当前时间、系统已运行时间、当前登录用户数量和平均负载

vmstat 用来获得有关进程、虚存、页面交换空间及 CPU活动的信息。这些信息反映了系统的负载情况。

top    查看系统整体状态

df -h  查看磁盘使用情况

du -h  查看文件夹大小

free   查看内存大小，可以使用 -m和-g来选择内存大小的单位

cat /proc/cpuinfo  查看cpu信息

cat /proc/meminfo  查看内存信息

cat /proc/loadavg  查看CPU的平均负载

cat /proc/5346/status  查看某个进程的状态，其中5346是pid，需要先找到进程的pid才行

find   在系统中查找指定类型或名字的文件

netstat  查看网络状况

sed    以行为单位进行数据的替换、删除、新增、选取等特定工作

awk    把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理

cut    从一个文本文件或者文本流中提取文本列

fdisk  划分磁盘空间用

ifconfig 配置网卡信息

route  配置路由信息

env    查看全部环境变量

export 设置环境变量的值

echo   可以打印指定的环境变量
