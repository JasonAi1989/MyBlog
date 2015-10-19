title: protocol —— 协议
toc: true
date: 2015-10-17 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### protocol是什么

protocol是objective-c中的一种约定，他不是类，只是在其声明中约定了相关的方法，然后再由遵守约定的类对其方法进行实现。

因为protocol不是类，所以本身没有对方法的实现，需要protocol的遵守者进行实现。

### protocol的使用方法

##### protocol的声明部分

```
@protocol ProtocolName

@required
- method1;

@optional
- method2;

@end
```
其中@required下面的方法是协议的遵守者必须实现的，而@optional下面的方法则是选择性实现的。默认情况下的方法是@required类型的。

##### 协议的遵守

可以在类声明时在ClassName后面使用<>将要遵守的协议扩起来，可以用逗号分隔多个协议

```
@interface ClassName : SuperClassName < protocol list >
@interface ClassName ( CategoryName ) < protocol list >
```

当需要判断协议中某个方法是否已经被实现了，可以使用如下方法进行判断：

```
respondsToSelector:@selector

if ( [anObject respondsToSelector:@selector(setOrigin::)] )  
    [anObject setOrigin:0.0 :0.0];  
```

其中setOrigin::是需要被测试的方法。

有了这样的声明，在.m文件中就可以放心的调用协议中声明的方法了。

##### 协议中方法的实现

在遵守协议的类实现中直接实现协议中的方法即可，和实现类中其他的方法没有差别

### protocol的使用场景

+ 定义对象的属性和行为
+ 用于回调

### 简单举例

.h
```
@protocol MyProtocol

@required
-(void) method1:
-(void) method2:

@optional
-(void) method3:

@end

@interface MyClass : NSObject <MyProtocol>

-(void) method4:

@end
```

.m
```
@impliment MyClass

-(void) method1
{}

-(void) method2
{}

-(void) method4
{}

@end
```

其他.m文件中
```
MyClass *mc = [[MyClass alloc] init];
[mc method1];
[mc method2];
[mc method3];
```

### 通过委托来使用protocol

委托（delegate），是把工作交给别人去做，其实就是主类中定义一个遵守收协议的id类型变量，当主类需要调用协议的方法时就调用这个代理变量中的方法，但具体是哪个类去实现的这个协议，主类是不关心的。有可能实现这个协议的就是这个主类本身，当然也可能是别的什么类。





















