title: iOS 自学
toc: true
date: 2015-09-05 22:15:37
tags: [Xcode, iOS]
categories: iOS 100 Days
description: iOS学习中遇到的一些记忆点以及一些问题的解决方法
feature: http://7xj4cp.com1.z0.glb.clouddn.com/Xcode_icon.png
---

+ 如果要在代码中定义一个控件，一般会把这个控件定义成let类型，也就是定义成常量；而一般的变量定义为普通变量即可。

+ objective c中基础数据类型有两类，一类是继承了c语言的基本数据类型，包括int/float/double/char/short int/long long int等；另一类是由FoundationKit提供的一些基础数据类型和基础数据类型类，例如NSInteger/NSNumber/NSString/NSMutableArray等。

<!-- more -->

+ Cocoa程序的编写主要要用到两个框架，Foundation和ApplicationKit(UIKit),其中Foundation框架主要定义了一些基础类，而ApplicationKit主要定义了一些用于Mac开发的基础类，而IOS的界面开发主要是用UIKit。

+ 在viewDidLoad 布局界面的元素时，如果布局无效，很可能是因为使用了autoLayout功能，需要取消此功能

![image](http://7xj4cp.com1.z0.glb.clouddn.com/ios-autoLayout.png)

+ [Spring Animation 简介，动画效果看过来。](http://www.tuicool.com/articles/ZR7nYv)

+ 将button的直角改为圆角，直接在界面中添加runtime attributes

![image](http://7xj4cp.com1.z0.glb.clouddn.com/ios-button.png)

+ 如果控件无法拖拽到源文件中从而产生对应的响应，可能是因为所拖拽的控件的类并非此源文件，开始学习时我就犯了这样的错误。

+ Foundation框架中的所有类都继承自NSObject，这就是所谓的上帝吧。Foundation主要提供了与图形用户界面没有直接关系的功能的一些类，比如：字符串、数值、容器集合等等相关的类。

+ oc中self和super的含义：

		self：当前对象

		super：上一层（从继承关系上讲）的类
		
+ oc中方括号的作用：	对象－>调用方法


		[a b]  其中a是对象，b是方法，此句的作用为：发送消息给对象a，使其执行方法b
		
+ 多态：父类类型的指针（引用）指向子类的对象。在定义时定义的为一个父类类型的变量，但在运行时此变量接受到的却是一个子类类型的对象，这时这个变量实际代表的就是子类对象

+ 放大和缩小storyBoard中的视图

		shift+command+option+{    zoom in
		shift+command+option+}    zoom out
		shift+command+option+|    reset
		
+ Xcode6如何设置storyboard中Controller的开始箭头
![image](http://7xj4cp.com1.z0.glb.clouddn.com/is_initial_view_controller.jpg)
![image](http://7xj4cp.com1.z0.glb.clouddn.com/is_initial_view_controller2.jpg)


		

