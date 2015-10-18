title: protocol —— 协议
toc: true
date: 2015-10-17 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### protocol是什么

protocol是objective-c中的一种约定，他不是类，只是在其声明中约定了相关的方法，然后再在某个遵守约定的类中对其方法进行实现。

因为protocol不是类，所以本身没有对方法的实现，需要protocol的遵守者进行实现。

### protocol的使用方法

protocol的声明部分

```
@protocol ProtocolName

@required
- method1;

@optional
- method2;

@end
```
其中@required下面的方法是协议的遵守者必须实现的，而@optional下面的方法则是选择性实现的。默认情况下的方法是@required类型的。

协议的采用，可以在类声明时在ClassName后面使用<>将要采用的协议扩起来，可以用逗号分隔多个协议

```
@interface ClassName : SuperClassName < protocol list >
@interface ClassName ( CategoryName ) < protocol list >
```

协议的采用者需要判断协议中某个方法是否已经被实现了，可以使用如下方法进行判断：

```
respondsToSelector:@selector

if ( [anObject respondsToSelector:@selector(setOrigin::)] )  
    [anObject setOrigin:0.0 :0.0];  
```

### protocol的使用场景

### 通过委托来使用protocol






















