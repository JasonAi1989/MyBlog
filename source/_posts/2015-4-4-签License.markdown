---
layout: post
title: 签License
categories: 笔记
tags: 远为
---
*This is a work note when I work in YuanV*

    /usr/src/feactl/src
    ./enckey 9VME13QF11111111 LD-L 13300000 128 10bf 99999999
其中9VME13QF11111111为序列号+n位1，凑成总共16位的字符串，序列号来源

    /vload/bin/smartctl -i /dev/ad4
其中/dev/ad4为硬盘的id，使用ls /dev/查看硬盘id

在shell中执行

    license a7de4d5a-4f6fcc07-5019f6dd-add5dd7e-d4079def-00000128-13300000-99999999
    
如果失败，可能是feactlinit的原因，在vload目录下执行可执行文件feactlinit

    /vload/bin/feactlinit
    
与此同时再次执行

    license a7de4d5a-4f6fcc07-5019f6dd-add5dd7e-d4079def-00000128-13300000-99999999