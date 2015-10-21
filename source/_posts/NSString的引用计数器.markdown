title: NSString的引用计数器
toc: true
date: 2015-10-22 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

在好多语言中字符串都是一个特殊的对象，在ObjC中也不例外。NSString作为一个对象类型存储在堆中，多数情况下它跟一般的对象类型没有区别，但是这里我们需求强调一点那就是字符串的引用计数器。

<!--more-->

```
#import <Foundation/Foundation.h>

int main(int argc,char *argv[]){
    
    NSString *str1=@"Kenshin";
    NSLog(@"retainCount(str1)=%i",(unsigned long)str1.retainCount); //结果：-1
    [str1 retain];
    NSLog(@"retainCount(str1)=%i",(unsigned long)str1.retainCount); //结果：-1
    
    NSString *str2=[NSString stringWithString:@"Kaoru"];
    NSLog(@"retainCount(str2)=%i",str2.retainCount); //结果：-1
    [str1 retain];
    NSLog(@"retainCount(str2)=%i",str2.retainCount); //结果：-1
    NSString *str2_1=[NSString stringWithString:[NSString stringWithFormat:@"Kaoru %@",@"sun"]];
    NSLog(@"retainCount(str2_1)=%i",str2_1.retainCount);//结果：2 
    [str2_1 release];
    [str2_1 release];
    
    
    NSString *str3=[NSString stringWithFormat:@"Rosa %@",@"Sun"];
    NSLog(@"retainCount(str3)=%i",str3.retainCount); //结果：1
    [str3 retain];
    NSLog(@"retainCount(str3)=%i",str3.retainCount); //结果：2
    [str3 release];
    [str3 release];
    
    NSString *str4=[NSString stringWithUTF8String:"Jack"];
    NSLog(@"retainCount(str4)=%i",str4.retainCount); //结果：1
    [str4 retain];
    NSLog(@"retainCount(str4)=%i",str4.retainCount); //结果：2
    [str4 release];
    [str4 release];
    
    NSString *str5=[NSString stringWithCString:"Tom" encoding:NSUTF8StringEncoding];
    NSLog(@"retainCount(str5)=%i",str5.retainCount); //结果：1
    [str5 retain];
    NSLog(@"retainCount(str5)=%i",str5.retainCount); //结果：2
    [str5 release];
    [str5 release];  
    
    NSMutableString *str6=@"Jerry";
    NSLog(@"retainCount(str6)=%i",str6.retainCount); //结果：-1
    [str6 retain];
    NSLog(@"retainCount(str6)=%i",str6.retainCount); //结果：-1
    [str6 release];
    [str6 release];
    
    NSMutableArray *str7=[NSMutableString stringWithString:@"Lily"];
    NSLog(@"retainCount(str7)=%i",str7.retainCount); //结果：1
    [str7 retain];
    NSLog(@"retainCount(str7)=%i",str7.retainCount); //结果：2
    [str7 release];
    [str7 release];
   
    return 0;
}
```

看完上面的例子如果不了解NSString的处理你也许会有点奇怪（注意上面的代码请在Xcode5下运行）？请看下面的解释

    1.str1是一个字符串常量，它存储在常量区，系统不会对它进行引用计数，因此无论是初始化还是做retain操作其引用计数器均为-1；
    2.str3、str4、str5创建的对象同一般对象类似，存储在堆中，系统会对其进行引用计数；
    3.采用stringWithString定义的变量有些特殊，当后面的字符串是字符串常量，则它本身就作为字符串常用量存储（str2），类似于str1；如果后面的参数是通过类似于str3、str4、str5的定义，那么它本身就是一个普通对象，只是后面的对象引用计数器默认为1，当给它赋值时会做一次拷贝操作（浅拷贝），引用计数器加1，所有str2_1引用计数器为2；
    4.str6其实和str1类似，虽然定义的是可变数组，但是它的本质还是字符串常量，事实上对于可变字符串只有为字符串常量时引用计数器才为-1，其他情况它的引用计数器跟一般对象完全一致；

**Tips**

	注意上面这段代码的运行结果是在Xcode5中运行的结果，事实上针对最新的Xcode6由于LLVM的优化，只有str2_1和str7的引用计数器为1（str7 retain一次后第二次为2），其他均为-1。