title: objective-c 基本数据类型
toc: true
date: 2015-10-15 22:15:37
tags: [objective-c, iOS]
categories: iOS 100 Days
description: 介绍objective-c中的基本数据类型
feature: http://7xj4cp.com1.z0.glb.clouddn.com/objective-c.png
---

### 基本数据类型
objective-c兼容c的基本数据类型，包括 int, short, long, double, float, char, bool(这个应该是c++的基本数据类型了)。

objective-c自己的基本数据类型，包括但并不只有 NSInteger, NSNumber, NSString, NSArray, NSSet, NSDictionary。

**Tips 1**

    NSInteger和NSNumber都表示整数，但前者是long的封装，而后者是类。在有些情况下只能使用NSNumber的这种整数类型。
    
**Tips 2**

    对比一下swift中的基本数据类型：Int, Double, Float, String, Character, Array, Set, Dictionary, Tuple(swift独有数据类型)
    
**Tips 3**

    在c语言中有void* 这种指针类型，可用于指定任意类型的指针；
    相似的，在objective-c中有id类型，这种类型的对象可以转换成任何一种对象；
    对比swift，在swift中有AnyObject类型，这种类型的对象也是可以转换成任何一种对象；
    
### NSNumber的常见操作

``` 
//创建数据类型：
numberWithInteger
numberWithLong
numberWithChar
numberWithFloat
numberWithDouble

//调用方式：
NSNumber *intNumber = [NSNumber numberWithInteger:123];

//比较：
isEqualToNumber
compare

//初始化实例  
NSNumber *number = [[NSNumber alloc] initWithInt:1000];  
```

### NSString的常见操作

NSString被称作不可修改字符串，一旦被创建就无法再修改其长度和内容。

NSString是以@符开始的字符串。

``` 
//初始化实例
NSString *str =@"我是字符串";  

//格式化字符串
stringWithFormat

NSString *name =@"zhang";  
NSString *log = [NSString stringWithFormat:@"I am '%@'",name];  
NSLog(@"str:%@",log); 

//初始化2
stringWithString

NSString *str =@"我是字符串";  
NSString *str1 = [NSString stringWithString:str]; 

//比较
isEqualToString
hasPrefix
hasSuffix

//数值转换
-(int) intValue;  
-(double) doubleValue;  
-(NSInteger) integerValue;  
-(float) floatValue; 

//大小写转换
-(NSString*) lowercaseString; //转换为不写的字符串  
-(NSString*) uppercaseString; //转换为大写的字符串  

//字符串截取
-(NSString*)substringFromIndex:i; //返回从i开始到结尾的子符串  
-(NSString*)substringToIndex:i;   //返回从字符串开始到i的字符串  
-(NSString*)substringWidthRange:range; //返回返回范围的字符串

//长度
-(UNSigned int)length; 

//char*的字符串转换为NSString字符串 
char *string = "我是字符串";    
NSString *Nstring = [[NSString alloc] initWithUTF8String:string]; 

//将NSString字符串得到char*字符串 
NSString *str=@"我是字符串";  
char *cStr = [str UTF8String];  
```

### NSMutableString的常见操作

NSString不允许修改字符串，可以使用NSMutableString，这种类型的字符串是允许被修改的。

NSMutableString是NSString类型的子类，因此所有适合于NSString的函数都适用于NSMutableString。

NSMutableString所特有的修改字符串的函数：

``` 
//创建一个字符串，容量为size大小
+(id)stringWithCapacity:size 	
-(id)initWithCapacity:size

//将字符串设置为 nsstring
-(void)setString:nsstring 	

//在字符串末尾追加字符串 nsstring
-(void)appendString:nsstring 	

//删除指定range 中的字符
-(void)deleteCharatersInRange:range 	

//以索引 i 为起始位置插入 nsstring
-(void)insertString:nsstring atIndex:i 	

//使用 nsstring 替换 range 指定的字符
-(void)replaceCharatersInRange;range withString:nsstring 	

//根据选项 opts ，使用指定 range 中的nsstring2 替换所有的 nsstring
-(void)replaceOccurrencesOfString:nsstring withString:nsstring2 options:opts range:range 	
```

### NSArray的常见操作

**Tips**

+ NSArray是不可变的数组类型
+ 只能储存Object-c对象，但可以同时存储不同类型的对象
+ 如果想存储原始的c数据，可以使用NSNumber类进行封装
+ 数组的最后一个元素一定是nil，表示结束。
+ 因为是不可变数组，所以没有插入、删除、替换等函数
+ 数组是连续的集合

``` 
//创建数组
NSArray *array1 = [NSArray arrayWithObjects:string1,string2, nil];
NSArray *array2 = [NSArray arrayWithArray:array1];

//创建数组只包含已有数组的一部分
NSRange range = NSMakeRange(0, 2);
NSArray *subArray = [array1 subarrayWithRange:range];

//array的长度
int arrayLength = [array1 count];

//访问数组中特定位置的一个对象
NSString *string = [array1 objectAtIndex:0];

//是否包含指定对象
BOOL isInArray = [array1 containsObject:string1];

//对象在数组中的位置
int index = [array1 indexOfObject:string1];

//遍历一个数组中的值
for(NSString *obj in array1)
{
    NSLog(@"%@",obj);
}

//反向遍历一个数组的值
for(NSString *objfan in [array1 reverseObjectEnumerator])
{
    NSLog(@"%@",objfan);
}

//将字符串切分成数组 componentsSeparatedByString:
NSString *str = @"oop : ack : elf : com : cn : net";
NSArray *arr = [str componentsSeparatedByString:@" : "];

//合并数组并创建字符串 componentsJoinedByString:
str = [arr componentsJoinedByString:@" , "];
```

### NSMutableArray的常见操作

NSMutableArray是NSArray的子类，用来提供可被修改的数组。

``` 
//向动态数组中添加
[myarray addObject:string3];
[myarray addObject:string2];
[myarray insertObject:string3 atIndex:0];
[myarray insertObject:string2 atIndex:1];

//替换
[myarray replaceObjectAtIndex:1 withObject:string3];

//删除
[myarray removeObject:string3];

//删除特定位置对象
[myarray removeObjectAtIndex:0];

//删除几个对象
[myarray removeObjectsInRange:range];

//删除所有对象
[myarray removeAllObjects];
```

### NSSet的常见操作

NSSet是和NSArray比较相似的集合，不同点是NSSet是无序的，而NSArray是有序的。

NSSet是无序的不可修改集合，其对元素的查找速度比NSArray快（采用hash查找）。

``` 
+(id)setWithObjects:obj1,obj2,...nil 	//使用一组对象创建新的集合
-(id)initWithObjects:obj1,obj2,....nil 	//使用一组对象初始化新分配的集合
-(NSUInteger)count 	                    //返回集合成员个数
-(BOOL)containsObject:obj 	            //确定集合是否包含对象 obj
-(BOOL)member:obj 	                    //确定集合是否包含对象 obj
-(NSEnumerator*)objectEnumerator 	    //返回集合中所有对象到一个 NSEnumerator 类型的对象
-(BOOL)isSubsetOfSet:nsset 	            //判断集合是否是NSSet的子集
-(BOOL)intersectsSet:nsset 	            //判断两个集合的交集是否至少存在一个元素
-(BOOL)isEqualToSet:nsset 	            //判断两个集合是否相等
```

### NSMutableSet的常见操作

NSMutableSet是NSSet的子类，可以被修改。

``` 
-(id)setWithCapcity:size 	//创建一个有size大小的新集合
-(id)initWithCapcity:size 	//初始化一个新分配的集合，大小为size
-(void)addObject:obj 	    //添加对象 obj 到集合中
-(void)removeobject:obj 	//从集合中删除对象 obj
-(void)removeAllObjects 	//删除集合中所有对象
-(void)unionSet:nsset 	    //将nsset的所有元素添加到集合
-(void)minusSet:nsset 	    //从集合中去掉所有的NSSet 的元素
-(void)interectSet:nsset 	//集合和NSSet 做交集运算
```

### NSDictionary的常见操作

NSDictionary可以将数据以键值对儿的形式储存起来，取值的时候通过KEY就可以直接拿到对应的值，非常方便。

和NSSet一样，NSDictionary也是无序的。

``` 
//创建字典
NSDictionary *dic1 = [NSDictionary dictionaryWithObject:@"value" forKey:@"key"];
NSDictionary *dic2 = [NSDictionary dictionaryWithObjectsAndKeys:  
                              @"value1", @"key1",  
                              @"value2", @"key2",  
                              @"value3", @"key3",  
                              @"value4", @"key4",  
                              nil]; 
NSDictionary *dic3 = [NSDictionary dictionaryWithDictionary:dic2];                       

//根据key获取value  
[dic3 objectForKey:@"key3"]
                              
//获取字典数量                               
dic3.count   

//所有的键集合 
NSArray *keys = [dic3 allKeys];      

//所有值集合  
NSArray *values = [dic3 allValues];                        
                              
```

### NSMutableDictionary的常见操作

NSMutableDictionary是可以被修改的字典类型。

```
//创建字典
NSMutableDictionary *dictMutable = [[NSMutableDictionary alloc] initWithObjectsAndKeys:m_array,@"sort",n_array,@"number", nil];  
 
//修改对象  
[dictMutable setObject:string4 forKey:@"sort"];  

//删除对象  
[dictMutable removeObjectForKey:@"number"];  

//删除多个对象  
NSArray *key_array =[NSArray arrayWithObjects:@"sort",@"number", nil];  
[dictMutable removeObjectForKey:key_array];  

//删除所有对象  
[dictMutable removeAllObjects];

```

### 三种集合类型

NSArray, NSSet, NSDictionary这三种集合类型（以及相应的可变类型）很相似，下面是这三种集合类型大体上的异同。

相同点：

+ 元素只能是对象类型，不能是int等基础类型，如果要使用如int等基础类型，需要使用NSNumber进行转换后使用

+ 一个集合里面的元素可以是不同类型的。

+ 都是不可变类型，并都有对应的可变类型的子类

不同点：

+ NSArray中的元素是有序排列，而NSSet, NSDictionary中的元素是无序排列

+ NSArray可以通过下标索引对应的值，NSSet只能通过便利索引对应的值，NSDictionary可以通过key进行索引

+ NSSet类型更适合做集合的操作，如求交集／并集等