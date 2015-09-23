title: Swift basic grammar 1
toc: true
date: 2015-09-06 14:46:42
tags: [Swift, iOS]
categories: iOS 100 Days
description: This is the Swift basic grammar.
feature: http://7xj4cp.com1.z0.glb.clouddn.com/swift_icom.jpg
---


[test.rar](http://7xj4cp.com1.z0.glb.clouddn.com/test.rar)
### 基础变量类型
Int, Float, Double

这三种基础类型可以直接通过显示类型转换来进行转换，swift是不接受隐式类型转换的

<!-- more -->

```
var a: Int = 1
var b: Double = 2.1
var c: Float = 3.2

a = Int(b)
a = Int(c)

a = 1
b = Double(a)
b = Double(c)  //注意这里会在精度上有一些问题,因为Double的精度比Float的高

b = 2.1
c = Float(a)
c = Float(b)   //但这里并不会有精度问题
```
如果想让这三种类型变成String类型，有两种简单的方法

```
var a: Int = 1
var b: Double = 2.1
var c: Float = 3.2
var d:String = "test"

d = "\(a) " + "\(b) " + "\(c) "
d = String(a) + String(stringInterpolationSegment: b) + String(stringInterpolationSegment: c)
```
第二种方式类似显示类型转换，但其实那只是字符串的一种初始化方式，在下面的字符串部分会详细介绍。

### 字符串 String/字符 Character
#### 初始化

```
var str = "hello world"
var str1:String?   //nil string
var str2 = ""      //empty string
var str3 = String()  //empty string, same as str2

str1?.isEmpty  //nil
str2.isEmpty   //true

//通过其他类型的变量进行转换
var str4 = String(stringInterpolationSegment: true)
//用这种方式几乎可以把所有基础类型转换成String类型
//extension String : StringInterpolationConvertible {
//    init(stringInterpolationSegment expr: String)
//    init(stringInterpolationSegment expr: Character)
//    init(stringInterpolationSegment expr: UnicodeScalar)
//    init(stringInterpolationSegment expr: Bool)
//    init(stringInterpolationSegment expr: Float32)
//    init(stringInterpolationSegment expr: Float64)
//    init(stringInterpolationSegment expr: UInt8)
//    init(stringInterpolationSegment expr: Int8)
//    init(stringInterpolationSegment expr: UInt16)
//    init(stringInterpolationSegment expr: Int16)
//    init(stringInterpolationSegment expr: UInt32)
//    init(stringInterpolationSegment expr: Int32)
//    init(stringInterpolationSegment expr: UInt64)
//    init(stringInterpolationSegment expr: Int64)
//    init(stringInterpolationSegment expr: UInt)
//    init(stringInterpolationSegment expr: Int)
//}

//其中String和Int类型的转换可以直接进行
var str5 = String("hello")
var str6 = String(11)
```
*注：Character和String类型都是使用双引号，所以很多时候这两个类型不会分的那么清楚，但Character永远都是一个字符，而不会成为多个*

#### 使用
+ String可以直接使用 ＋ 操作，但Character不可以使用 ＋ 操作

+ 除了 ＋ 操作外还能使用append方法进行字符和字符串的追加

+ 可以使用for-in方式进行字符串的遍历

+ 可以使用countElements()进行字符串长度的计算

+ 字符串的比较，直接使用关系运算符即可，按字典序进行比较

```
var str_a = "abc"
var str_b = "abc"

str_a == str_b  //true

var str_c = "ab"

str_a == str_c  //false
str_a > str_c   //true

var str_d = "abd"

str_a < str_d  //true
```
+ 字符串前缀和后缀

```
var nameArray = ["第一章 1.基本环境介绍",
                "第二章 1.整型",
                "第二章 2.浮点型",
                "第二章 3.字符串",
                "第二章 4.Optional",
                "第二章 5.nil聚合",
                "第三章 1.算术运算符",
                "第三章 2.逻辑运算符",
                "第三章 3.位运算符"]

var countPrefix=0, countSuffix=0
for name in nameArray
{
    if name.hasPrefix("第二章")
    {
        countPrefix++
    }
    
    if name.hasSuffix("运算符")
    {
        countSuffix++
    }
}

countPrefix  //5
countSuffix  //3
```

+ 字符串大小写

```
str.capitalizedString  //首字母大写
str.uppercaseString    //所有字母大写
str.lowercaseString    //所有字母小写
```

+ 字符串去处开头和结尾处的特殊符号

```
//trim  去除字符串前面和后面的字符，不能去除中间的
var str1 = "   hello!  "
str1.stringByTrimmingCharactersInSet(NSCharacterSet.whitespaceCharacterSet()) //hello!  去除了空格
str1.stringByTrimmingCharactersInSet(NSCharacterSet(charactersInString: " !")) //hello   去除了空格和叹号，用此方法可以去除所提供的所有字符
```

+ 字符串分割

```
//split  分割
var str2 = "Hello Jason and Jane"
str2.componentsSeparatedByString(" ")

var str3 = "Hello Jason and M-J-Alex"
str3.componentsSeparatedByCharactersInSet(NSCharacterSet(charactersInString: " -"))

```

+ 字符串连接

```
//join  连接
var str4 = ["Hello", "I", "am", "Jason"] //被连接起来的字符串
var str5 = "-" //用于连接的字符串
str5.join(str4)  //"Hello-I-am-Jason"
```
+ 在String中使用Range

```
str = "English is a West Germanic language that was first spoken in early medieval England and is now a global lingua franca"

//range
str.rangeOfString("West") //13..<17
str.rangeOfString("west", options: NSStringCompareOptions.CaseInsensitiveSearch) //13..<17

//String.index  index是String类中的一个结构体,下面这两个变量就是index这个结构体类型的拥有计算属性的变量
str.startIndex //0
str.endIndex   //117

//创建一个Range类型的变量
let strRange = Range <String.Index> (start:str.startIndex, end:str.endIndex) //0..<117

//advance用于计算一个新位置，返回值为第一个参数的类型，第一个参数为起始位置，第二个参数为偏移量
let newEndIndex:String.Index = advance(str.startIndex, 14)  //14  返回的不是一个Int型，而是一个String.Index类型
let newStrRange = Range <String.Index> (start:str.startIndex, end:newEndIndex) //0..<14

//发现并返回给定范围内第一次出现的给定的字符串的位置范围
str.rangeOfString("west", options: NSStringCompareOptions.CaseInsensitiveSearch, range: newStrRange)

//substring
var newEndIndex2 = advance(str.startIndex, 9)
str.substringToIndex(newEndIndex) //返回从起始位置到给定位置的字符串

var newStartIndex2 = advance(str.startIndex, 3)
str.substringFromIndex(newStartIndex2) //返回从给定位置到结束位置的字符串

//返回给定范围的字符串
str.substringWithRange(Range <String.Index>(start: newStartIndex2, end: newEndIndex2))
```

+ 插入、删除和替换

```
//insert a character 这个会改变str的原始值
str.insert("H", atIndex: str.startIndex)
str

//remove  这个会改变str的原始值
str.removeAtIndex(advance(str.startIndex, 10))
str

//replace
str.replaceRange(Range <String.Index>(start: advance(str.startIndex, 5), end: advance(str.startIndex, 12)), with: " again ")
```

### 数组 Array
#### swift中数组与OC中数组的区别：

+ swift数组中只能存储某一种类型的元素

+ swift数组中存储的可以是基础元素，没必要非得是类元素

#### 初始化

```
var array1 = ["a", "b", "c", "d", "f"]
var array2:[String]?          //nil
var array3:Array<String>?     //nil
var array4 = Array<String>()  //empty
var array5 = []               //empty

array2?.isEmpty   //nil
array4.isEmpty    //true
```

#### 基本操作

```
var array = ["A", "B", "C", "D", "E"]

array.append("F")
array += ["G", "H"]

array.insert("hello", atIndex: 0)
array.removeAtIndex(3)
array.removeLast()

array[1] = "KO"
array[2...5] = ["new"]  //将2...5元素删除，然后添加新赋的值
```

#### 遍历

```
//遍历索引
for index in 0..<array.count
{
    println(array[index])
}

//遍历元素
for item in array
{
    println(item)
}

//遍历索引和元素
for (index, item) in enumerate(array)
{
    println("\(index)-\(item)")
}
```

### 字典 Dictionary
#### swif中字典和OC中字典的区别（和数组差不多）

+ swift中一个字典只能存储一种键和值的固定搭配
	
+ swift中字典所存储的数据中，键和值可以是任意类型数据

#### 初始化
```
var dict1 = [22:"Hello", 44:"Jason", 33:"World"]
var dict2:Dictionary<Int, String>?      //nil
var dict3:[Int:String]?                 //nil
var dict4 = [:]                         //empty
var dict5 = Dictionary<Int, String>()   //empty

dict2?.isEmpty  //nil
dict5.isEmpty   //ture
```

*注：数组和字典中使用［］声明的空数组、字典，最好在前面加上具体的变量类型*

#### 基本操作
```
dict1.count
dict1.isEmpty

dict1[12] = "New" //add key/value
dict1[12] = nil   //del key/value
dict1[12]  //没有访问越界的限制

dict1.removeValueForKey(22)  //return the old value
dict1.updateValue("Kate", forKey: 44)  //replace, and return the old value
```

#### 遍历
```
for (key, value) in dict1
{
    println("\(key)-\(value)")
}

dict1.keys  //it likes an array, and we can use Array( dic1.keys ) to translate a real array
dict1.values  //like blove up
Array(dict1.keys)
Array(dict1.values)

for key in dict1.keys
{
    println(key)
}

for value in dict1.values
{
    println(value)
}
```

### 集合 Set
+ 集合(Set)用来存储相同类型并且没有确定顺序的值。当集合元素顺序不重要时或者希望确保每个元素只出现一次时可以把集合当做是数组另一形式。

+ 集合和字典存储的类型必须是可哈希化的，Swift 的所有基本类型(比如String,Int,Double和Bool)默认都是可哈希化的

＋ Set类型可以使用数组字面量进行赋值，但必须要注明变量的类型为Set，否则会被推断成数组类型

+ Set类型是无序的，但可以使用类中的sort函数进行相关的排序

#### 初始化
```
var set: Set<String> = ["Rock", "Classical", "Hip hop"]
var set2 = Set<Character>()  //empty
var set3: Set<String> = []   //empty

var set4: Set = ["Rock", "Classical", "Hip hop"]  //it's OK, 可以通过Set类型和数组字面量类型推断出具体的Set类型
```

#### 基本操作
```
set.isEmpty
set.count
set.contains("Hey")
set.insert("You")
set.remove("Rock")
```

#### 遍历
```
for item in set
{
    print(item)
}
```
#### 集合操作
![集合操作](http://7xj4cp.com1.z0.glb.clouddn.com/Set.png)


+ 使用“是否等”运算符(==)来判断两个集合是否包含全部相同的值。
+ 使用isSubsetOf(\_:)方法来判断一个集合中的值是否也被包含在另外一个集合中。
+ 使用isSupersetOf(\_:)方法来判断一个集合中包含的值是另一个集合中所有的值。
+ 使用isStrictSubsetOf(\_:)或者isStrictSupersetOf(\_:)方法来判断一个集合是否是另外一个集合的子集合或者父集合并且和特定集合不相等。
+ 使用isDisjointWith(\_:)方法来判断两个结合是否不含有相同的值。


### 元组 Tuple
+ Tuple能放不同类型的元素，而Array只能放相同类型的元素

```
var tup = (flag: true, name:"Jason")
var (flag, name) = tup
var (flag1, _) = tup  //不关心的参数可以使用 _ 接收

tup.flag
tup.name

flag
name

flag1
```
*注：元组这个数据类型和之前的有些区别，好像无法定义一个空的元组*
