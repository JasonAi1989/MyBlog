title: block —— 代码块
toc: true
date: 2015-10-21 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

ObjC中的Block是对闭包的实现，而闭包的主要作用就是实现c语言中的回调函数的特性。说到回调函数的特性，protocol+委托功能不也是对回调函数的一种实现嘛，所以在某些场合Block是能替换protocol+委托功能，但如果要实现的方法比较多还是用protocol+委托的方式来实现吧。

<!--more-->
KCButton.h

```
#import <Foundation/Foundation.h>
@class KCButton;
typedef void(^KCButtonClick)(KCButton *);

@interface KCButton : NSObject

@property (nonatomic,copy) KCButtonClick onClick;
//上面的属性定义等价于下面的代码
//@property (nonatomic,copy) void(^ onClick)(KCButton *);

-(void)click;
@end
```

KCButton.m

```
#import "KCButton.h"

@implementation KCButton

-(void)click{
    NSLog(@"Invoke KCButton's click method.");
    if (_onClick) {
        _onClick(self);
    }
}

@end
```

main.m

```
#import <Foundation/Foundation.h>
#import "KCButton.h"

int main(int argc, const char * argv[]) {

    KCButton *button=[[KCButton alloc]init];
    button.onClick=^(KCButton *btn){
        NSLog(@"Invoke onClick method.The button is:%@.",btn);
    };
    [button click];
    /*结果：
     Invoke KCButton's click method.
     Invoke onClick method.The button is:<KCButton: 0x1006011f0>.
     */
    
    return 0;
}
```

关于Block总结如下：

    1.Block类型定义：返回值类型(^ 变量名)(参数列表)（注意Block也是一种类型）；
    2.Block的typedef定义：返回值类型(^类型名称)(参数列表)；
    3.Block的实现：^(参数列表){操作主体}；
    4.Block中可以读取块外面定义的变量但是不能修改，如果要修改那么这个变量必须声明_block修饰；












