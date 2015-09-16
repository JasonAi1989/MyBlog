title: ospf配置
toc: true
date: 2015-07-09 18:31
tags: [freeBSD] [ospf]
categories: freeBSD
description:
feature:
---

ospf需要多台机器之间配合才能有用，但这里只介绍一台机器的配置情况，其它机器只需修改某些特殊信息，需要修改的信息会用加粗字体来标注。

<!-- more -->

1、配置zebra
在使ospf生效之前必须先开启并配置zebra。下面为从shell中黏贴过来的信息。

    load1# /vload/bin/zebra -d -f /vload/etc/zebra.conf -u root -g wheel
    load1# telnet localhost 2601
    
    Password: 
    Router> en
    Password: 
    Router# con t
    Router(config)# interface fxp0
    Router(config-if)# ip address 211.88.25.226/24 
    Router(config-if)# wri file
    Configuration saved to /vload/etc/zebra.conf

**注：**

**1、其中第一行的程序运行需要根据实际情况进行修改**

**2、fxp0需要修改为对应的网卡名，具体可以使用ifconfig进行查看**

**3、ip地址需要是你本机对应网卡上的IP地址**

2、配置ospf
    
    load1# /vload/bin/ospfd -d -f /vload/etc/ospf.conf -u root -g wheel
    load1# telnet localhost 2604

    Password: 
    ospfd> en
    ospfd# con t
    ospfd(config)# router ospf 
    ospfd(config-router)# network 211.88.25.226/24 area 1
    ospfd(config-router)# wri mem
    Configuration saved to /vload/etc/ospf.conf
    
**注：**

**1、其中第一行的程序运行需要根据实际情况进行修改**

**2、IP地址为需要向外发布的IP地址**

**3、area后面跟的ID值，在同一网段中的路由，此ID值必须一样，否则即便收到ospf hello包，同样是无法将其设置成neighbors**

3、验证是否设置成功

在跟命令行下，命令：

    ospfd# show ip ospf 
      border-routers  for this area
      database        Database summary
      interface       Interface information
      neighbor        Neighbor list
      route           OSPF routing table
      
可以查看多项信息，只有当接收到相同area id值的ospf hello包时才能将其设置成neighbors。