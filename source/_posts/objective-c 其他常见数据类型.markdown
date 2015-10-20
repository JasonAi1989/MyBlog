title: objective-c 其他常见数据类型
toc: true
date: 2015-10-16 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 介绍objective-c中的基本数据类型
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### 其他常见数据类型

这里介绍几个其他几个常见的数据类型，包括 NSEnumerator, NSData, NSDate等。

常用的结构体有NSRange、NSPoint、NSSize、NSRect等。

### NSEnumerator

NSEnumerator是一个抽象类，这个类型的实例只能通过集合类型变量的相关方法进行初始化。简单的说，这个类型本身没有初始化函数，需要通过集合类型（NSArray, NSSet, NSDictionary）中的相关方法进行初始化。

NSEnumerator类型的变量可以通过调用nextObject方法迅速获取到集合中的元素，当遍历完集合中的所有元素后会返回nil

```
//通过NSArray创建实例
- (NSEnumerator<ObjectType> *)objectEnumerator
- (NSEnumerator<ObjectType> *)reverseObjectEnumerator

//通过NSSet创建实例
- (NSEnumerator<ObjectType> *)objectEnumerator

//通过NSDictionary创建实例
- (NSEnumerator *)keyEnumerator
- (NSEnumerator<ObjectType> *)objectEnumerator

//获取下一个元素
- (ObjectType)nextObject
```

### NSData

NSData和它的子类NSMutableData提供数据对象，这两个数据类型典型应用场景是在数据存储上，同时在分布式对象应用中也很有帮助。

NSData和他的子类可以快速简便的将内容存储到磁盘上，是数据丢失的风险降到最低，这个类型提供了一些列自动存储数据的方法。

自动写入最开始会将数据写入到一个临时文件中，当写入成功后才会将这个临时文件移动到其最后的位置上。

虽然自动写入功能降低了因文件损坏而带了的风险，但并不是说自动写入功能是完全安全的，尤其当写入的位置是一个临时目录，用户的home目录或者是任意一个公开的容易访问的位置时。任何时候，当你访问一个公开的容易访问的文件时，都应该把其看作是一个不可信的文件，或者拥有潜在风险的文件。黑客可能会破坏此文件，或者通过软硬链接使你改写一个系统文件。

当要写入的位置是一个公开的容易访问的目录时，应该避免使用writeToURL:atomically:等相关方法。可以使用 NSFileHandle对象取而代之。这个对象会更加安全。更多信息可访问[ Securing File Operations ](https://developer.apple.com/library/mac/documentation/Security/Conceptual/SecureCodingGuide/Articles/RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW9)

### NSDate

NSDate对象用来表示一个具体的时间点。

```
//初始化
+ dateWithTimeIntervalSinceNow:
+ dateWithTimeInterval:sinceDate:
+ dateWithTimeIntervalSinceReferenceDate:
+ dateWithTimeIntervalSince1970:
- initWithTimeIntervalSinceNow:
- initWithTimeInterval:sinceDate:
- initWithTimeIntervalSinceReferenceDate:
Designated Initializer
- initWithTimeIntervalSince1970:

//比较日期
- isEqualToDate:
- earlierDate:
- laterDate:
- compare:

//将日期转化为字符串
description
- descriptionWithLocale:
```

代码举例：

```
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    
    //获得当前日期
    NSDate *date1=[NSDate date];
    NSLog(@"%@",date1); 
    
    //在当前日期的基础上加上100秒，注意在ObjC中多数时间单位都是秒
    NSDate *date2=[NSDate dateWithTimeIntervalSinceNow:100];
    NSLog(@"%@",date2); 
    
    //随机获取一个将来的日期
    NSDate *date3=[NSDate distantFuture];
    NSLog(@"%@",date3);
    
    //日期之差,返回单位为秒
    NSTimeInterval time=[date2 timeIntervalSinceDate:date1];
    NSLog(@"%f",time); 
    
    //返回比较早的日期
    NSDate *date5=[date1 earlierDate:date3];
    NSLog(@"%@",date5); 
    
    //日期格式化
    NSDateFormatter *formater1=[[NSDateFormatter alloc]init];
    formater1.dateFormat=@"yy-MM-dd HH:mm:ss";
    NSString *datestr1=[formater1 stringFromDate:date1];
    NSLog(@"%@",datestr1); 
    
    //字符串转化为日期
    NSDate *date6=[formater1 dateFromString:@"14-02-14 11:07:16"];
    NSLog(@"%@",date6);

    return 0;
}
```

### 常用结构体

下面用一段代码来解释常用的四种结构体。

```
#import <Foundation/Foundation.h>

/*NSRange表示一个范围，(arg1, arg2) => (location, length)*/
void test1(){
    NSRange rg1={3,5};

    NSRange rg2;
    rg2.location=3;
    rg2.length=5;

    NSRange rg3={.location=3,.length=5};

    //常用下面的方式定义 
    NSRange rg4=NSMakeRange(3,5);
    
    //打印NSRange可以使用Foundation中方法 
    NSLog(@"rg2 is %@", NSStringFromRange(rg4));
    
    //注意不能直接NSLog(@"rg4 is %@", rg4)，因为rg4不是对象（准确的说%@是指针）而是结构体
}

/*NSPoint表示一个点, (arg1, arg2) => (x, y)*/
void test2(){
    NSPoint p=NSMakePoint(10, 15);
    
    //NSPoint其实就是CGPoint
    //这种方式比较常见 
    CGPoint p2=CGPointMake(10, 15);
    
    NSLog(NSStringFromPoint(p2));
}

/*NSSize表示大小, (arg1, arg2) => (width, height)*/
void test3(){
    NSSize s=NSMakeSize(10, 15);
    
    //NSSize其实就是CGSize
    //这种方式比较常见 
    CGSize s2=CGSizeMake(10, 15);

    NSLog(NSStringFromSize(s2));
}

/*NSRect表示一个矩形, (arg1, arg2, arg3, arg4) => (x, y, width, height)*/
void test4(){
    NSRect r=NSMakeRect(10, 5, 100, 200);
    
    //NSRect其实就是CGRect
    //这种方式比较常见 
    CGRect r2=CGRectMake(10, 5, 100, 200);

    NSLog(NSStringFromRect(r2));
}

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        test1();
        test2();
        test3();
        test4();
    } return 0;
}
```

**Tips**

    1.对于常用结构体在Foundation框架中都有一个对应的make方法进行创建；
    2.对应的还都有一个NSStringFromXX方法来进行字符串转换，方便我们调试；
    3.结构体类型的变量不能用"*"来接收；


### 官方文档

Foundation框架介绍性的文章自然不少，这里给出[Foundation框架的官方文档](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/ObjC_classic/index.html#//apple_ref/doc/uid/20001091)。


Foundation框架所提供的很多类都具有相似的功能。

+ Data storage —— 数据存储
    - NSData, NSString 提供对二进制和字符串的存储
    - NSValue, NSNumber 提供对c语言数据类型的存储
    - NSArray, NSDictionary, NSSet 提供对对象类型数据的存储
 
+ Text and strings —— 文本和字符串
    - NSCharacterSet 可以设置NSString 和 NSScanner 类中所使用的字符
    - NSString 类可以展现文本和字符串，并提供方法进行字符串的查找、组合和比较
    - NSScanner 类可以浏览NSString对象中的数字和单词
    
+ Dates and times —— 日期和时间
    - NSDate, NSTimeZone, NSCalendar 存储时间和日期，并且展示日历信息，提供用于计算时间和日期不同的方法
    - NSLocale 提供方法来显示世界上不同地域所以用的不同日期格式
    
+ Application coordination and timing —— 应用协同和定时
    - NSNotification, NSNotificationCenter, NSNotificationQueue 提供一个通知系统
    - NSTimer  用于定时发送消息
    
+ Object creation and disposal —— 对象创建和消除
    - NSAutoreleasePool  类用于延时释放功能
    
+ Object distribution and persistence —— 对象分布和保持
    - NSPropertyListSerialization
    - NSCoder 

+ Operating-system services —— 操作系统服务
    - NSFileManager 提供固定的接口用于文件操作（文件的创建/删除/重命名等）
    - NSThread, NSProcessInfo 可以创建多线程的应用，并查询应用所运行的环境
    
+ URL loading system —— URL加载系统






















