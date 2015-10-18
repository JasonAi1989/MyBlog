title: category
toc: true
date: 2015-10-19 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 介
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### category是什么

category是ios进行类扩展的一种方式，同样能对类尽兴扩展的方式还有extension和继承（inherit）。

extension是可以看作是一种匿名的category方式，具体异同会在后文详述。

继承（inherit）是通过实现子类来对父类尽兴扩展。

### category的使用方法

头文件中的声明方式

```
@interface ClassName (CategoryName)  
-methodName1  
-methodName2  
@end  
```

.m文件中的实现方式

```
@implementation ClassName (CategoryName)  
-methodName1  
-methodName2  
@end  
```

上文是对ClassName类进行扩展，小括号中的CategoryName是分类的名字。

### category的优点与缺点

category：

+ 无法添加新的实例变量

+ 可以添加新的方法

+ 新添加的方法可以只声明，不实现

+ 可以重写原始类中的方法

extension：

+ 可以添加新的实例变量

+ 可以添加新的方法

+ 新添加的方法声明后必须实现，不然会有编译错误

+ 可以重写原始类中的方法

inherit：

+ 可以添加新的实例变量

+ 可以添加新的方法

+ 新添加的方法声明后必须实现，不然会有编译错误

+ 可以重写父类中的方法

### Category扩展方式的使用场景

Category可以在如下场景中使用：

+ 类包含了很多个方法实现，而这些方法需要不同团队的成员来实现
+ 当你在使用基础类库中的类时，你不想继承这些类而只想添加一些方法时。
 
Category使用时需要注意的问题：

+ Category可以访问原始类的实例变量，但不能添加实例变量，如果想添加变量，那就通过继承创建子类来实现。
+ Category可以重载原始类的方法，但是不推荐这么做，这样会覆盖掉原始类的方法。如果确实要重载，那就通过继承创建子类来实现。
+ 和普通接口有所区别的是，在Category的实现文件中的实例方法只要你不去调用它你可以不用实现所声明的方法。

### 其他方式的扩展

这里介绍一下extension的扩展方式：

头文件中的声明方式

```
@interface ClassName(){
    int iextension;
}

-methodName1  
-methodName2 

@end
```

.m文件中的实现方式

```
@implementation ClassName

-methodName1  
-methodName2 

@end
```


















