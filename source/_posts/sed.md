title: sed笔记
toc: true
date: 2015-04-05 14:46:42
tags: [shell, sed]
categories: shell
description: 
---

## 替换并输出（不修改源文件）：

    sed  's/dog/cat/g' file      ##dog被替换的内容，cat替换的内容

## 备份后直接替换至源文件：

    sed -i.bak 's/dog/cat/g' file  ##g意味着全局替换
<!--more-->
## 替换第n行到第m行：

    sed 'n,ms/dog/cat/g' file  ##n、m为数字

## 替换内容xxx和***之间的内容：

    sed '/A/,/B/s/dog/cat/g' file  ##替换A和B之间的内容

## 一次替换多个多个内容：

    sed  -e  's/dog1/cat1/g' -e  's/dog2/cat2/g' file

## 查询
    sed -n "/#telnet/p" $file

## 当需要使用变量时要用双引号，如下
    newfile=$(echo $file | sed -e "s/$src/$tar/g")

## 处理多行
sed是处理行的工具，因此一般情况下是无法处理多行的，可以如下使用

    sed 'N;s///g'
其中添加N;即可处理多行，比如替换换行符\n等
但此种方法并不好用，有些行会无法替换

## 用sed在所有行的结尾添加字符
    sed 's/$/abcd/g' file
$代表所有行的行尾
可以使用此方式来进行行末尾字符的替换，先在每行末尾添加特殊字符，然后将要替换的字符和特殊字符进行匹配，最后去掉所有的特殊字符
