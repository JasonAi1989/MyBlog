title: objective-c KVC与KVO
toc: true
date: 2015-10-21
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### 概述

KVC和KVO都应算是动态特性。

+ KVC（Key Value Coding）键值编码
+ KVO（Key Value Observing）键值监测

<!--more-->

### KVC

KVC可以通过键的方式给一个变量赋值（获取值），并且**无论这个变量是否为private均可访问**。

这个索引用的键一般就是变量的字面名字，比如需要访问的变量为a，则键为@“a”。

通过KVC设置变量可以使用下面的方法：

```
setValue:属性值 forKey:属性名        //用于简单路径
setValue:属性值 forKeyPath:属性路径  //用于复合路径,例如person.account
```

通过KVC读取变量可以使用下面的方法：

```
valueForKey:属性名        //用于简单路径
valueForKeyPath:属性名    //用于复合路径
```

例如：

```
[person setValue:@28 forKey:@"age"];
[person valueForKey:@"age"];

[person setValue:@100000000.0 forKeyPath:@"account.balance"];
[[person valueForKeyPath:@"account.balance"] floatValue];
```

age为person类中的一个私有变量，account是person类中的一个属性变量，account类中还有一个变量balance。

**Tips**

    1.即便是类中的私有变量，通过KVC也能进行访问；
    2.如果是访问类中变量中的变量，需要使用复合路径；
    3.如果要赋值的变量不是字符串类型，也要在第一个参数中调用@符号，@+具体值，但如果值是一个对象，那就直接写对象名就好了。
    
KVC进行变量查找的规则（以查找变量a为例）：

+ 如果是变量赋值操作
    + 优先调用类中属性变量a的setter方法；
    + 如果没有a的setter方法，则试图访问_a变量；
    + 如果没有_a变量，则试图访问a变量；
    + 如果没有a变量，则会调用这个类的setValue: forUndefinedKey：方法

+ 如果是变量取值操作
    + 优先调用类中属性变量a的getter方法；
    + 如果没有a的getter方法，则试图访问_a变量；
    + 如果没有_a变量，则试图访问a变量；
    + 如果没有a变量，则会调用这个类的valueforUndefinedKey: 方法

使用KVC方式进行值访问的目的：简化代码。

例如，变量a=@"name"，此时如果想获取类中name变量的值就可以直接通过KVC获取

```
value = [person valueForKey:a];
```

不用KVC的代码如下：

```
if([a isEqualToString:@"name"])
{
    value = person.name;
}
```

### KVO

Key Value Observing（简称KVO）,键值监听，简单来说，就是当某个被监听的值发生变化时，会通知相应的监听函数，监听者可以重写这个监听函数，从而得到变动通知。

KVO使用方法：

+ 注册监听值 
    + 
```    
addObserver: forKeyPath: options:  context:
```
+ 删除监听值
    + 
```
removeObserver: forKeyPath
```
    + 
```
removeObserver: forKeyPath: context:
```
+ 回调监听
    + 
```
observeValueForKeyPath: ofObject: change: context:
```

KVO使用步骤：

    1.通过addObserver: forKeyPath: options: context:为被监听对象（它通常是数据模型）注册监听器；
    2.重写监听器的observeValueForKeyPath: ofObject: change: context:方法






