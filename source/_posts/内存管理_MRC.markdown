title: objective-c 内存管理 —— MRC
toc: true
date: 2015-10-20 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### 引用计数

在objective-c中没有类似JAVA中的垃圾回收器，但也不像c语言那样直接释放。

在每个对象中都有一个retainCount的变量用来对对象的引用进行计数。

可以增加retainCount的方法有：

<!--more-->

+ alloc
+ retain
+ new
+ copy

可以减少retainCount的方法有：

+ release

### MRC 手动引用计数

在MRC模式下，当某个对象有新的引用时，需要使用[object retain]方法来手动的增加retainCount的值；而当某个对象不需要某个引用时，则需要使用[object release]方法来手动减少retainCount的值。

当retainCount的值为0时，会自动调用dealloc释放对象。

[object release]方法只会将引用计数的值-1，并不会将引用所使用的对象指针赋值为nil，所以在调用此方法后需要手动给对象指针赋值为nil，以防出现野指针。

### @autorelease 延迟释放

在MRC的基础上发展出了@autorelease延迟自动释放功能，注意，此功能和后文所说的ARC功能是不一样的。

如果想使用延时释放功能，需要两个条件：

+ 拥有@autorelease代码块
+ 对象调用了autorelease方法

此功能只是在调用autorelease方法时将对象加入到@autorelease代码块的release队列中，当执行到代码块的后大括号时，将队列中的所有对象一次调用release方法进行释放。所以了autorelease方法只是做了一个延迟操作。

需要注意的是，当了@autorelease代码块结束时并不会将代码块中对象的引用计数清零，如果对象的引用计数大于1，当代码块结束后，对象仍然拥有大于0的引用计数值。

一句话，autorelease方法只是将对象的1次release操作延迟到了代码块结束时。

@autorelease代码块可以嵌套，但是一个对象不能调用多次autorelease方法，否则可能会引起问题。

**总结**

    1.autorelease方法不会改变对象的引用计数器，只是将这个对象放到自动释放池中；
    2.自动释放池实质是当自动释放池销毁后调用对象的release方法，不一定就能销毁对象（例如如果一个对象的引用计数器>1则此时就无法销毁）；
    3.由于自动释放池最后统一销毁对象，因此如果一个操作比较占用内存（对象比较多或者对象占用资源比较多），最好不要放到自动释放池或者考虑放到多个自动释放池；
    4.ObjC中类库中的静态方法一般都不需要手动释放，内部已经调用了autorelease方法；

### @property 属性变量

@property可以创建属性变量，相当于swift中的计算属性，可以创建一个拥有getter和setter方法的变量。

@property后面可以跟三个参数，用于指定创建的属性变量的具体属性，语法规则如下：

```
@property(arg1, arg2, arg3) ObjectType *ObjectName;
```

其中参数arg1代表原子性；arg2代表读写属性；arg3代表set方法处理。

具体可选值如下表：

![property](http://7xj4cp.com1.z0.glb.clouddn.com/property.png)

示例如下：

```
@property (nonatomic, readwrite, retain) NSNumber *num;
```

**Tips**

    1.retain，通常用于非字符串对象
    2.copy，通常用于字符串对象、block、NSArray、NSDictionary
    
当给一个属性变量赋值一个对象时，不需要再手动的调用retain方法给这个对象的引用计数+1，但是仍需要使用release方法进行释放。















