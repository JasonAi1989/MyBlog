title: iOS 三种事件控制
toc: true
date: 2015-11-4 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

![ios evnets](http://7xj4cp.com1.z0.glb.clouddn.com/ios_events.png)

<!--more-->

1. 触摸事件：通过触摸、手势进行触发（例如手指点击、缩放）
2. 运动事件：通过加速器进行触发（例如手机晃动）
3. 远程控制事件：通过其他远程设备触发（例如耳机控制按钮）

### 触摸事件

继承UIResponder的对象均能接收触摸事件，而常用的UIView、UIViewController、UIApplication都继承自UIResponder，因此，基本上所有的UIKit控件和视图控制器均能接收触摸事件。

触摸事件所需要的方法：

```
//一根或多根手指开始触摸屏幕时执行；
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event;

//一根或多根手指在屏幕上移动时执行，注意此方法在移动过程中会重复调用；
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event;

//一根或多根手指触摸结束离开屏幕时执行；
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;

//触摸意外取消时执行（例如正在触摸时打入电话）；
- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event;
```

当执行触摸事件时，上述函数会传入 (NSSet *)touches对象，这个集合中每一个元素都是
UITouch类型的，通过

```
UITouch *touch=[touches anyObject];
```

获取一个或者多个触摸对象。

UITouch类中包涵如下属性：

- window：触摸时所在的窗口
- view：触摸时所在视图
- tapCount:短时间内点击的次数
- timestamp:触摸产生或变化的时间戳
- phase:触摸周期内的各个状态
- locationInView:方法：取得在指定视图的位置
- previousLocationInView:方法：取得移动的前一个位置

#### 响应者链

在视图的多层架构中，可以按照视图的叠加层次进行链式响应，如果当前视图无法对触摸事件进行响应，事件的响应者会变为当前视图的包涵视图，以此类推，从而使事件的响应者形成一个链，叫做响应者链。如果响应者链中的某个响应者处理了事件，则此事件不会继续向下传播；反之则会一直向下传播，直到此事件被丢弃为止。

![response chain](http://7xj4cp.com1.z0.glb.clouddn.com/response_chain.png)

响应者链中的响应者不能处理事件的条件：

+ userInteractionEnabled=NO
+ hidden=YES
+ alpha=0~0.01
+ 没有实现开始触摸方法（注意是touchesBegan:withEvent:而不是移动和结束触摸事件）

当然前三点都是针对UIView控件或其子控件而言的，第四点可以针对UIView也可以针对视图控制器等其他UIResponder子类。对于第四种情况这里再次强调是对象中重写了开始触摸方法，则会处理这个事件，如果仅仅写了移动、停止触摸或取消触摸事件（或者这三个事件都重写了）没有写开始触摸事件，则此事件该对象不会进行处理。

#### 手势识别系统

##### 6种基本手势类
UIKit封装了6种基本手势，分别为：

|手势类|说明|
|---|---|
|UITapGestureRecognizer| 	点按手势|
|UIPinchGestureRecognizer| 	捏合手势|
|UIPanGestureRecognizer| 	拖动手势|
|UISwipeGestureRecognizer| 	轻扫手势，支持四个方向的轻扫，但是不同的方向要分别定义轻扫手势|
|UIRotationGestureRecognizer| 	旋转手势|
|UILongPressGestureRecognizer| 	长按手势|

##### 手势类中的基本属性和方法

所有的手势操作都继承于UIGestureRecognizer，这个类本身不能直接使用。这个类中定义了这几种手势共有的一些属性和方法(下表仅列出常用属性和方法)：

|名称|说明|
|---|---|
|属性||	 
|@property(nonatomic,readonly) UIGestureRecognizerState state; | 手势状态|
|@property(nonatomic, getter=isEnabled) BOOL enabled; |手势是否可用|
|@property(nonatomic,readonly) UIView \*view; 	|触发手势的视图（一般在触摸执行操作中我们可以通过此属性获得触摸视图进行操作）|
|@property(nonatomic) BOOL delaysTouchesBegan; |手势识别失败前不执行触摸开始事件，默认为NO；如果为YES，那么成功识别则不执行触摸开始事件，失败则执行触摸开始事件；如果为NO，则不管成功与否都执行触摸开始事件；|
|方法 	 ||
|- (void)addTarget:(id)target action:(SEL)action; |添加触摸执行事件|
|- (void)removeTarget:(id)target action:(SEL)action; |移除触摸执行事件|
|- (NSUInteger)numberOfTouches; |	触摸点的个数（同时触摸的手指数）|
|- (CGPoint)locationInView:(UIView \*)view; 	|在指定视图中的相对位置|
|- (CGPoint)locationOfTouch:(NSUInteger)touchIndex inView:(UIView*)view; |触摸点相对于指定视图的位置|
|- (void)requireGestureRecognizerToFail:(UIGestureRecognizer *)otherGestureRecognizer; 	|指定一个手势需要另一个手势执行失败才会执行|
|代理方法 	 ||
|- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer; 	|一个控件的手势识别后是否阻断手势识别继续向下传播，默认返回NO；如果为YES，响应者链上层对象触发手势识别后，如果下层对象也添加了手势并成功识别也会继续执行，否则上层对象识别后则不再继续传播；|

##### 手势的状态

手势的状态分为以下6种：

```
typedef NS_ENUM(NSInteger, UIGestureRecognizerState) {
    UIGestureRecognizerStatePossible,   // 尚未识别是何种手势操作（但可能已经触发了触摸事件），默认状态
    
    UIGestureRecognizerStateBegan,      // 手势已经开始，此时已经被识别，但是这个过程中可能发生变化，手势操作尚未完成
    UIGestureRecognizerStateChanged,    // 手势状态发生转变
    UIGestureRecognizerStateEnded,      // 手势识别操作完成（此时已经松开手指）
    UIGestureRecognizerStateCancelled,  // 手势被取消，恢复到默认状态
    
    UIGestureRecognizerStateFailed,     // 手势识别失败，恢复到默认状态
    
    UIGestureRecognizerStateRecognized = UIGestureRecognizerStateEnded // 手势识别完成，同UIGestureRecognizerStateEnded
};
```

在6种基本手势中，点按属于离散型手势，其它的五种属于连续手势。离散手势无法被cancel，而且执行结果要么是fail，要么是end。下面为苹果官方的手势状态示意图，各个圆圈分别代表手势的各种状态。

![gesture state](http://7xj4cp.com1.z0.glb.clouddn.com/gesture_state.png)

下面是离散型手势和连续型手势触发效果图：

![discrete continuous gesture](http://7xj4cp.com1.z0.glb.clouddn.com/discrete_continuous_gesture.png)

##### 手势系统的使用方式

下面是手势系统的使用步骤：

1. 创建对应的手势对象；
2. 设置手势识别属性【可选】；
3. 附加手势到指定的对象；
4. 编写手势操作方法；

下面的代码段简述了如何使用点按手势，其它手势的使用与此基本相同，不同的是需要设置的手势识别属性。

```
/*添加点按手势*/
//创建手势对象
UITapGestureRecognizer *tapGesture=[[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(tapGestureAction:)];

//设置手势属性
tapGesture.numberOfTapsRequired=1;//设置点按次数，默认为1，注意在iOS中很少用双击操作
tapGesture.numberOfTouchesRequired=1;//点按的手指数

//添加手势到对象
[self.view addGestureRecognizer:tapGesture];
```

**Tips**

	1. 真正在点按时处理业务操作的函数是 tapGestureAction:
	2. 添加手势的对象（上面代码段中为self.view）需要满足处理事件的四个条件（前文已经介绍过这四个条件了），否则添加的手势无法被触发。
	
##### 手势冲突

轻扫和拖动容易产生手势冲突，拖动和长按也会产生手势冲突，解决此问题的方法就是使用方法

```
- (void)requireGestureRecognizerToFail:(UIGestureRecognizer *)otherGestureRecognizer;
```

比如如下代码段：

```
//解决在图片上滑动时拖动手势和轻扫手势的冲突，当拖动执行失败时才执行轻扫
[panGesture requireGestureRecognizerToFail:swipeGestureToRight];
[panGesture requireGestureRecognizerToFail:swipeGestureToLeft];

//解决拖动和长按手势之间的冲突，当长按执行失败时才执行拖动
[longPressGesture requireGestureRecognizerToFail:panGesture];
```

##### 多个控件均处理同一个事件

上面说过，在响应者链中如果一个响应者已经处理了事件，事件就不会继续向后传递，但是，有时需要多个控件对同一个事件作出响应，这时我们就需要做一些操作使事件被处理后仍能继续向后传递。

```
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer;
```

使用上面这个代理方法即可实现。这个方法磨人返回NO，阻止继续向后传递事件，如果返回YES便可以继续向后传递事件。

这个代理方法在协议 UIGestureRecognizerDelegate  中。

给需要向后继续传递的手势对象指定代理：

```
UILongPressGestureRecognizer *viewLongPressGesture=[[UILongPressGestureRecognizer alloc]initWithTarget:self action:@selector(longPressView:)];
    viewLongPressGesture.delegate=self;
```

下面的代码段对代理方法进行实现：

```
-(BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer{    
    //注意，这里控制只有在UIImageView中才能向下传播，其他情况不允许
    if ([otherGestureRecognizer.view isKindOfClass:[UIImageView class]]) {
        return YES;
    }
    return NO;
}
```
此方法的第二个参数是将要被阻断的手势，然后可以通过判断这个手势的view控件是什么类型或者和其它控件的地址是否相等等方式，确定这个手势是否需要被传递下去。

### 运动事件

相比于触摸事件，运动事件的使用比较简单，下面是运动事件使用的三个方法：

```
//运动开始时执行；
- (void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event NS_AVAILABLE_IOS(3_0); 	

//运动结束后执行；
- (void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event NS_AVAILABLE_IOS(3_0); 	

//运动被意外取消时执行；
- (void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event NS_AVAILABLE_IOS(3_0); 	
```
    
监听运动事件对于UI控件有个前提就是监听对象必须是第一响应者（对于UIViewController视图控制器和UIAPPlication没有此要求）。这也就意味着如果监听的是一个UI控件那么

```
-(BOOL)canBecomeFirstResponder;
```
方法必须返回YES(可通过重写控件的此方法实现)。同时控件显示时调用视图控制器的becomeFirstResponder方法。当视图不再显示时注销第一响应者身份。

**Tips**

	1. 控件显示时注册第一响应者可以通过重写	
		-(void)viewWillAppear:(BOOL)animated; 或者
		-(void)viewDidAppear:(BOOL)animated; 方法实现
	2. 控件不显示时注销第一响应者可以通过重写
		-(void)viewDidDisappear:(BOOL)animated; 或者
		-(void)viewWillDisappear:(BOOL)animated; 方法实现
	
因此，在运动事件中需要父视图和子控件配合完成。

子控件中重写

```
-(BOOL)canBecomeFirstResponder;
- (void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event 
```

父视图中重写

```
-(void)viewWillAppear:(BOOL)animated; 或者
-(void)viewDidAppear:(BOOL)animated; 

-(void)viewDidDisappear:(BOOL)animated; 或者
-(void)viewWillDisappear:(BOOL)animated; 
```

### 远程控制事件

远程控制事件这里主要说的就是耳机线控操作。

和远程控制有关的只有下面这一个函数：

```
//接收到远程控制消息时执行；
- (void)remoteControlReceivedWithEvent:(UIEvent *)event NS_AVAILABLE_IOS(4_0); 
```

要监听到这个事件有三个前提（视图控制器UIViewController或应用程序UIApplication只有两个）：


1. 启用远程事件接收（使用[[UIApplication sharedApplication] beginReceivingRemoteControlEvents];方法）。
2. 对于UI控件同样要求必须是第一响应者（对于视图控制器UIViewController或者应用程序UIApplication对象监听无此要求）。
3. 应用程序必须是当前音频的控制者，也就是在iOS 7中通知栏中当前音频播放程序必须是我们自己开发程序。

远程控制事件只能用来控制音频，如果我们开发的应用和音频无关，但又想使用远程控制，可能需要我们在内部自己实现一个音频功能。

运动事件以及远程控制事件的事件类型：

```
typedef NS_ENUM(NSInteger, UIEventSubtype) {
    // available in iPhone OS 3.0
    UIEventSubtypeNone                              = 0,
    
    // for UIEventTypeMotion, available in iPhone OS 3.0
    UIEventSubtypeMotionShake                       = 1,
    
    // for UIEventTypeRemoteControl, available in iOS 4.0
    UIEventSubtypeRemoteControlPlay                 = 100,
    UIEventSubtypeRemoteControlPause                = 101,
    UIEventSubtypeRemoteControlStop                 = 102,
    UIEventSubtypeRemoteControlTogglePlayPause      = 103,
    UIEventSubtypeRemoteControlNextTrack            = 104,
    UIEventSubtypeRemoteControlPreviousTrack        = 105,
    UIEventSubtypeRemoteControlBeginSeekingBackward = 106,
    UIEventSubtypeRemoteControlEndSeekingBackward   = 107,
    UIEventSubtypeRemoteControlBeginSeekingForward  = 108,
    UIEventSubtypeRemoteControlEndSeekingForward    = 109,
};
```

**Tips**

	1. 为了模拟一个真实的播放器，程序中我们启用了后台运行模式，配置方法：在info.plist中添加UIBackgroundModes并且添加一个元素值为audio。
	2. 即使利用线控进行音频控制我们也无法监控到耳机增加音量、减小音量的按键操作（另外注意模拟器无法模拟远程事件，请使用真机调试）。
	3. 子事件的类型跟当前音频状态有直接关系，点击一次播放/暂停按钮究竟是【播放】还是【播放/暂停】状态切换要看当前音频处于什么状态，如果处于停止状态则点击一下是播放，如果处于暂停或播放状态点击一下是暂停和播放切换。 