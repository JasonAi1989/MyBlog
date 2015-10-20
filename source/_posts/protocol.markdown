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
<!--more-->

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

### 代码举例

KCButton.h

```
#import <Foundation/Foundation.h>
@class KCButton;

//一个协议可以扩展另一个协议，例如KCButtonDelegate扩展了NSObject协议
@protocol KCButtonDelegate <NSObject>

@required //@required修饰的方法必须实现
-(void)onClick:(KCButton *)button;

@optional //@optional修饰的方法是可选实现的
-(void)onMouseover:(KCButton *)button;
-(void)onMouseout:(KCButton *)button;

@end

@interface KCButton : NSObject

//代理属性，同时约定作为代理的对象必须实现KCButtonDelegate协议
@property (nonatomic,retain) id<KCButtonDelegate> delegate;

//点击方法
-(void)click;

@end
```

KCButton.m

```
#import "KCButton.h"

@implementation KCButton

-(void)click{
    NSLog(@"Invoke KCButton's click method.");
    
    //判断_delegate实例是否实现了onClick:方法（注意方法名是"onClick:",后面有个:）
    //避免未实现ButtonDelegate的类也作为KCButton的监听
    if([_delegate respondsToSelector:@selector(onClick:)]){
        [_delegate onClick:self];
    }
}

@end
```

MyListener.h

```
#import <Foundation/Foundation.h>
@class KCButton;
@protocol KCButtonDelegate;

@interface MyListener : NSObject<KCButtonDelegate>
-(void)onClick:(KCButton *)button;
@end
```

MyListener.m

```
#import "MyListener.h"
#import "KCButton.h"

@implementation MyListener
-(void)onClick:(KCButton *)button{
    NSLog(@"Invoke MyListener's onClick method.The button is:%@.",button);
}
@end
```

main.m

```
#import <Foundation/Foundation.h>
#import "KCButton.h"
#import "MyListener.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        
        KCButton *button=[[KCButton alloc]init];
        MyListener *listener=[[MyListener alloc]init];
        button.delegate=listener;
        [button click];
        /* 结果：
         Invoke KCButton's click method.
         Invoke MyListener's onClick method.The button is:<KCButton: 0x1001034c0>.
         */
    }
    return 0;
}
```


**Tips**

    1.id可以表示任何一个ObjC对象类型，类型后面的”<协议名>“用于约束作为这个属性的对象必须实现该协议(注意：使用id定义的对象类型不需要加“*”);
    2.MyListener作为事件触发者，它实现了KCButtonDelegate代理（在ObjC中没有命名空间和包的概念，通常通过前缀进行类的划分，“KC”是我们自定义的前缀）;
    3.在.h文件中如果使用了另一个文件的类或协议我们可以通过@class或者@protocol进行声明，而不必导入这个文件，这样可以提高编译效率（注意有些情况必须使用@class或@protocol，例如上面KCButton.h中上面声明的KCButtonDelegate协议中用到了KCButton类，而此文件下方的KCButton类声明中又使用了KCButtonDelegate，从而形成在一个文件中互相引用关系，此时必须使用@class或者@protocol声明，否则编译阶段会报错），但是在.m文件中则必须导入对应的类声明文件或协议文件（如果不导入虽然语法检查可以通过但是编译链接会报错）；
    4.使用respondsToSelector方法可以判断一个对象是否实现了某个方法（需要注意方法名不是”onClick”而是“onClick:”，冒号也是方法名的一部分）；




















