title: Install git on Unix
toc: true
date: 2015-06-04 14:46:42
tags: [git]
categories: git 
description: git在ubuntu和freeBSD上的安装方式
feature: http://7xj4cp.com1.z0.glb.clouddn.com/git_icon.jpg
---

## git安装

在ubuntu上安装

    Sudo apt-get install git-core

在freebsd上安装

    gmake prefix=/usr/local all doc
    
<!--more-->
编译出现无法找到curl.h文件，原因是未安装curl,下载安装curl

    curl安装过程：
    ./configure
    make
    make install
    
编译出现缺少expat.h的警告，然后出现一堆编译错误，原因是未安装expat，下载expat

    expat安装过程：
    ./configure
    make
    make install
    
编译出现缺少Python，下载安装Python，安装过程同上

安装

    gmake prefix=/usr/local install install-doc install-html

设置git的全局变量
    
    git config --global user.name "Your Name"
    git config --global user.email you@email.com

设置其他一些参数，可以直接编辑git的配置文件，打开.gitconfig文件，配置如下

    [user]
        name = AiZhaoyu
        email = aizhaoyu@163.com
    [alias]
        st=status
        ci=commit -m
        br=branch
        co=checkout
        df=diff --raw
        lg=log --oneline

## Gitolite安装以及仓库建立（在ubuntu上安装）

安装gitolite    
    
    sudo apt-get install gitolite

安装ssh       

    sudo apt-get install openssh-server

建立仓库专用帐号    

    sudo adduser --system --shell /bin/bash --group git
    sudo adduser git ssh
    sudo passwd git
    
在客户机上生成ssh公钥   
    
    ssh-keygen
将客户机上生成的公钥 id_rsa.pub改名为name.pub，然后上传到（不管用什么方法）服务器的  /tmp目录下

生成管理员仓库（管理员仓库用于对其他所有仓库进行管理与控制）

    gl-setup   /tmp/aizhaoyu.pub   
    
在客户机上克隆管理员仓库    
    
    git  clone  git@servername:gitolite-admin
    
在客户机的gitolite-admin/conf目录下的gitolite.conf文件中添加需要建立的仓库；

在客户机的gitolite-admin/keydir目录下增加其他客户机的公钥；

添加之后将数据更新到服务器的gitolite-admin仓库中，便会自动创建所需要的仓库。

注：有时可能会需要git账户拥有sudo的权限，需要在可用sudo命令的账户下执行sudo visudo 命令，然后添加  git  ALL=(ALL:ALL)  ALL  这句话到文件中。

## Git命令
删除已经存储的文件   
    
    git rm -r --cached [file]

添加标签   
    
    git tag -a 
    
显示标签   
    
    git tag

## 添加远端版本库

    git remote add [name] url
    eg: git remote add origin ssh://git@211.88.25.109/loadpass.git