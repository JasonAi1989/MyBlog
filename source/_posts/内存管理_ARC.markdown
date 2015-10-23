title: objective-c 内存管理 —— ARC
toc: true
date: 2015-10-20 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### ARC 自动引用计数

简单地说，就是代码中自动加入了retain/release，原先需要手动添加的用来处理内存管理的引用计数的代码可以自动地由编译器完成了。

该机能在 iOS 5/ Mac OS X 10.7 开始导入，利用 Xcode4.2 可以使用该机能。简单地理解ARC，就是通过指定的语法，让编译器(LLVM 3.0)在编译代码时，自动生成实例的引用计数管理部分代码。有一点，ARC并不是GC，它只是一种代码静态分析（Static Analyzer）工具。

<!--more-->

在4.2版本之后的Xcode中，当创建项目时会自动使用ARC模式。

对整个工程取消使用ARC模式，可以将项目编译设置中的“Objectice-C Auto Reference Counteting”设为NO，如下所示。

![Cancle ARC](http://7xj4cp.com1.z0.glb.clouddn.com/MRC.png)

如果只是想给特定的文件取消ARC模式，可以只针对该类文件加上 -fno-objc-arc 编译FLAGS，如下图。

![Cancle ARC single](http://7xj4cp.com1.z0.glb.clouddn.com/MRC_single.png)

+ 打开ARC：-fobjc-arc

+ 关闭ARC：-fno-objc-arc

在非ARC模式下需要遵守的内存管理规则是：（采用autorelease方式）

+ 生成对象时，使用autorelease
+ 对象代入时，先autorelease后再retain
+ 对象在函数中返回时，使用return [[object retain] autorelease];

而使用ARC后，我们可以不需要这样做了，甚至连最基础的release都不需要了。

在ARC模式下内存管理的基本规则:

+ retain, release, autorelease, dealloc由编译器自动插入，不能在代码中调用
+ dealloc虽然可以被重载，但是不能调用[super dealloc]

使用ARC模式的好处：

+ 代码变得简单多了，因为我们不需要担心烦人的内存管理，担心内存泄露了
+ 代码的总量变少了，看上去清爽了不少，也节省了劳动力
+ 代码高速化，由于使用编译器管理引用计数，减少了低效代码的可能性

使用ARC模式的坏处：

+ 记住一堆新的ARC规则，关键字及特性等需要一定的学习周期
+ 一些旧的代码，第三方代码使用的时候比较麻烦；修改代码需要时间，要么修改编译开关

关于第二点，由于 XCode4.2 中缺省ARC就是 ON 的状态，所以编译旧代码的时候往往有"Automatic Reference Counting Issue"的错误信息。

### ARC修饰符

在ARC模式下有四种修饰符来定义变量的引用特性，分别为: 

    __strong
    __weak
    __autoreleasing
    __unsafe_unretained

@property的属性变量的修饰符也有所改变，如下：

![property ARC](http://7xj4cp.com1.z0.glb.clouddn.com/property_arc.png)

下面是这几个修饰符的介绍。

##### __strong

表示引用为强引用。对应在定义property时的"strong"。所有对象只有当没有任何一个强引用指向时，才会被释放。

**Tips**

    如果在声明引用时不加修饰符，那么引用将默认是强引用。当需要释放强引用指向的对象时，需要将强引用置nil。

##### __weak

表示引用为弱引用。对应在定义property时用的"weak"。

弱引用不会影响对象的释放，即只要对象没有任何强引用指向，即使有100个弱引用对象指向也没用，该对象依然会被释放。不过好在，对象在被释放的同时，指向它的弱引用会自动被置nil，这个技术叫zeroing weak pointer。这样有效得防止无效指针、野指针的产生。

__weak一般用在delegate关系中防止循环引用或者用来修饰指向由Interface Builder编辑与生成的UI控件。

##### __autoreleasing

表示在autorelease pool中自动释放对象的引用，和MRC时代autorelease的用法相同。定义property时不能使用这个修饰符，任何一个对象的property都不应该是autorelease型的。

一个常见的误解是，在ARC中没有autorelease，因为这样一个“自动释放”看起来好像有点多余。这个误解可能源自于将ARC的“自动”和autorelease“自动”的混淆。其实你只要看一下每个iOS App的main.m文件就能知道，autorelease不仅好好的存在着，并且变得更fashion了：不需要再手工被创建，也不需要再显式得调用[drain]方法释放内存池。

![ARC autorelease]()

以下两行代码的意义是相同的。

```
NSString *str = [[[NSString alloc] initWithFormat:@"hehe"] autorelease]; // MRC
NSString *__autoreleasing str = [[NSString alloc] initWithFormat:@"hehe"]; // ARC
```

**Tips 1**

    __autoreleasing在ARC中主要用在参数传递返回值（out-parameters）和引用传递参数（pass-by-reference）的情况下。
    
例如：

```
NSError *__autoreleasing error;
￼if (![data writeToFile:filename options:NSDataWritingAtomic error:&error]) 
￼{ 
　　NSLog(@"Error: %@", error); 
}
```

**Tips 2**

    如果你的error定义为了strong型，那么，编译器会帮你隐式地做如下事情，保证最终传入函数的参数依然是个__autoreleasing类型的引用。
    
```
NSError *error; 
NSError *__autoreleasing tempError = error; // 编译器添加 
if (![data writeToFile:filename options:NSDataWritingAtomic error:&tempError]) 
￼{ 
    error = tempError; // 编译器添加 
    NSLog(@"Error: %@", error); 
}
```

所以为了提高效率，避免这种情况，我们一般在定义error的时候将其声明为__autoreleasing类型。

**Tips 3**

    在ARC中，所有指针的指针 （NSError **）的函数参数如果不加修饰符，编译器会默认将他们认定为__autoreleasing类型。
    
例如：

```
- (NSString *)doSomething:(NSNumber **)value
{
        // do something  
}
```

除非你显式得给value声明了\_\_strong，否则value默认就是__autoreleasing的。

##### __unsafe_unretained

ARC是在iOS 5引入的，而这个修饰符主要是为了在ARC刚发布时兼容iOS 4以及版本更低的设备，因为这些版本的设备没有weak pointer system，简单的理解这个系统就是我们上面讲weak时提到的，能够在weak引用指向对象被释放后，把引用值自动设为nil的系统。这个修饰符在定义property时对应的是"unsafe_unretained"，实际可以将它理解为MRC时代的assign：纯粹只是将引用指向对象，没有任何额外的操作，在指向对象被释放时依然原原本本地指向原来被释放的对象（所在的内存区域）。所以非常不安全。

现在可以完全忽略掉这个修饰符了。

### 修饰符的使用语法

```
ClassName * qualifier variableName;

NSString * __weak str = @"hehe"; // 正确
__weak NSString *str = @"hehe";  // 错误
```

**Tips**
    
    1.在iOS开发中使用strong、weak代替之前的retain、assign（基本类型使用assign，因为我们不需要对基本类型进行内存管理）；
    2.如果一个属性使用IBOutlet修饰（也就是此属性是strongboard中组件）那么使用weak；
    3.如果一个属性不是storyboard组件（一般纯代码编写界面时），使用strong；










