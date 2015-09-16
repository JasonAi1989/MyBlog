title: Ruby学习记录——安装Ruby
toc: true
date: 2015-03-23 20:56
tags: [Ruby]
categories: Ruby
description:
feature:
---

在Ubuntu上安装Ruby，首先想到的是到官网下载源码包然后编译安装，但在安装过程中遇到各种编译和安装错误，貌似是缺少很多的依赖库，在网上搜了一下，有好多的依赖库要安装，于是选择了另一条路进行安装（Ubunt官方软件源的版本太老了，学习最好能安装最新稳定版本的）。

<!-- more -->

http://chloerei.com/2014/07/13/the-best-way-to-install-the-latest-version-of-ruby-on-ubuntu/

之后便按照这个网址上的方法进行安装，很轻松的搞定。

这里顺带补一下apt-add-repository的知识。
以前一直是使用Ubuntu各个服务器上的源（用的最多的是网易的服务器），而此条命令允许用户使用额外的有个人维护的源（PPA源）。
PPA 全称为 Personal Package Archives（个人软件包档案），是 Ubuntu Launchpad 网站提供的一项服务，当然不仅限于 Launchpad 。它允许个人用户上传软件源代码，通过 Launchpad 进行编译并发布为二进制软件包，作为 apt/新立得源供其他用户下载和更新。在Launchpad网站上的每一个用户和团队都可以拥有一个或多个PPA。

通常 PPA 源里的软件是官方源里没有的，或者是最新版本的软件。相对于通过 Deb 包安装来说，使用 PPA 的好处是，一旦软件有更新，通过 sudo apt-get upgrade 这样命令就可以直接升级到新版本。
新添加的PPA源最好想apt-get update一下，然后在安装要安装的软件。