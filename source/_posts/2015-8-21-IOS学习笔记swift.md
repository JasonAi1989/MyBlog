---
layout: post
title: IOS 学习笔记——swift
categories: 笔记
tags: IOS
---
###swift

+ 同c++一样，可以定义多个构造函数，不同构造函数的参数列表一定要不一样，
	- 构造函数的样子为：
	
			init(){
				code
			}


	- 析构函数的样子为：

			deinit{
				code
			}

	- 析构函数没有参数列表

+ 在子类中重写父类的方法需要用override，如果父类中有此方法，但是参数列表不一样，则不需要使用此关键字

+ 类中的变量拥有三种属性，分别为：存储属性／计算属性／类属性

	- 存储属性就是最基本的类变量，但是需要注意的是，这类变量需要赋初始值，否则就需要在init函数中进行初始化

	- 计算属性是拥有get和set方法的变量，样子为：
 
 			var sum:Int{
				get(){
					return self.a + self.b
	 			}
 			}
 			
		- 计算属性的变量不能赋初始值

	- 类属性，相当于c++中的静态变量，只能是此类调用这个变量，类实例化的变量无法调用这个变量，样子为

			class var description:String{
				get(){
					println(“Hello");
	 			}
 			}
 			
		- 类属性的变量必然是拥有get和set方法的

+ 变量用var定义，变量后面可用：type的方式声明具体的变量类型
+ 常量用let定义，具体类型的声明同var

	- 控件类型常被定义成let类型的

+ 布尔类型：

		OC－－YES,NO
		swift－－true，false

+ swift的if中不需要小括号，并且必须要有花括号
+ swift的if中的判断值需要是true或者false，或者是逻辑表达式，不能是0或者非0值

		var a = 7
		if a{
 		}
这时错误的写法，程序会直接报错

+ swift Tuple and Array

		Tuple能放不同类型的元素，而Array只能放相同类型的元素

+ 对于无兴趣的数值可以使用下滑线  _  接受，如

		let loginResult = ( true, “hello”)
		let (flag,  _ ) = loginResult

		if flag{
			code
		}
这里的flag接受了变量值true，下划线_接受了变量hello

+ 一个Tuple变量的定义和初始化方式：
	
		let loginResult:( Bool, String ) = (flag: true, name:”Hello”)
		
	- 调用：
		
			loginResult.0
			loginResult.1
	OR
	
			loginResult.flag
			loginResult.name

+ Int, Float, Double, Character, String, Array, Tuple, Optional

+ Optional ：或者是一个值，或者是没值。没有值时是nil

	？ ： 对某个类型封装为Optional类型

	！ ： 对Optional类型变量进行解包，从而成为真正的类型

+ OC中的nil只针对指针，而swift中的nil可以对Int等类型使用

+ swift中nil聚合运算符，是对Optioanl变量判断进行的一次简化，例如

```
var nickName:String?

nickName = "Jason"

//if nickName == nil{
//    println("Hello Guest")
//}else{
//    println("Hello \(nickName!)")
//}

var result = nickName ?? "Guest"
println("Hello \(result)")
```

+ 区间运算符

		闭区间运算符  ...
		半闭区间     ..< (新版本)    ..（老版本）
			
	- 老版本的半闭区间运算符已经被废除
	
+ 区间匀速符配合for-in循环

```
for index in 0...10
{
    index
    
    index = 3  //报错，原因是默认未声明的index为常量
}
```
想把index声明为变量的方法

```
for (var index) in 0...10
{
    index
    
    index = 3
}
```
下面的方式无法生效，会报错，原因是for循环中的index和for循环为声明的index不是同一个值，for循环中index的作用域为整个for循环，不受外界影响，同时不可在外部使用

```
var index:Int
for index in 0...10
{
    index
    
    index = 3
}
```
个人认为，这个特性很容易让程序员忽视，从而引起bug

+ 字符串／字符

```

var str1 = "Hello"
let str2 = "World"

str1 = "你好"
//str2 = "世界"  //错误的，这是个常量字符串

str1 += str2  //字符串可以用+=相连

var ch:Character = "!"  //定义一个字符变量

//str1 += ch //不能用 +=来处理字符
str1.append(ch)

for ch2 in str1  //可以用for-in来遍历字符串
{
    ch2
}

countElements(str1)  //计算字符串长度，swift中没有对字符串长度的一个类方法

//OC
var str_oc:NSString = "Hello World"
str_oc.length   //OC中有字符串长度的计算方法

var str3 = String()
var str4 = ""

str3.isEmpty  //true
str4.isEmpty  //true
```

+ 字符串比较（按字典序比较）

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

+ 使用Foundation库来扩展字符串的方法，需要引用此类库

```
import Foundation

var str = "Hello, playground"

str.capitalizedString  //首字母大写
str.uppercaseString    //所有字母大写
str.lowercaseString    //所有字母小写

str  //str的值并未改变，前三个方法并不会改变字符串原值

//trim  去除字符串前面和后面的字符，不能去除中间的
var str1 = "   hello!  "
str1.stringByTrimmingCharactersInSet(NSCharacterSet.whitespaceCharacterSet()) //hello!  去除了空格
str1.stringByTrimmingCharactersInSet(NSCharacterSet(charactersInString: " !")) //hello   去除了空格和叹号，用此方法可以去除所提供的所有字符

//split  分割
var str2 = "Hello Jason and Jane"
str2.componentsSeparatedByString(" ")

var str3 = "Hello Jason and M-J-Alex"
str3.componentsSeparatedByCharactersInSet(NSCharacterSet(charactersInString: " -"))

//join  连接
var str4 = ["Hello", "I", "am", "Jason"] //被连接起来的字符串
var str5 = "-" //用于连接的字符串
str5.join(str4)  //"Hello-I-am-Jason"
```

+ String.Index和Range

```
var str = "English is a West Germanic language that was first spoken in early medieval England and is now a global lingua franca"

//range
str.rangeOfString("West") //13..<17
str.rangeOfString("west", options: NSStringCompareOptions.CaseInsensitiveSearch) //13..<17

//String.index  这是一个数据类型
str.startIndex //0
str.endIndex   //117

let strRange = Range <String.Index> (start:str.startIndex, end:str.endIndex) //0..<117

let newEndIndex:String.Index = advance(str.startIndex, 14)  //14  返回的不是一个Int型，而是一个String.Index类型
let newStrRange = Range <String.Index> (start:str.startIndex, end:newEndIndex) //0..<14

str.rangeOfString("west", options: NSStringCompareOptions.CaseInsensitiveSearch, range: newStrRange) //nil

//substring
var newEndIndex2 = advance(str.startIndex, 9)
str.substringToIndex(newEndIndex) //"English is a W"

var newStartIndex2 = advance(str.startIndex, 3)
str.substringFromIndex(newStartIndex2)

str.substringWithRange(Range <String.Index>(start: newStartIndex2, end: newEndIndex2))  //"lish i"

//insert a character 这个会改变str的原始值
str.insert("H", atIndex: str.startIndex)
str

//remove  这个会改变str的原始值
str.removeAtIndex(advance(str.startIndex, 10))
str

//replace
str.replaceRange(Range <String.Index>(start: advance(str.startIndex, 5), end: advance(str.startIndex, 12)), with: " again ")
``` 

+ swift中数组与OC中数组的区别：
	- 一个数组中只能存储某一种类型的元素
	
	- 数组中存储的可以是基础元素，没必要非得是类元素
	
	
+ 数组初始化

```
//Array init 3 ways
var array1 = ["a", "b", "c", "d", "f"]
var array2:[String] = ["a", "b", "c", "d", "f"]
var array3:Array<String> = ["a", "b", "c", "d", "f"]

//define a empty array, 0 elements
var array4 = [] //don't recomment, there will be some errors, we don't know the element type
var array5 = [String]()
var array6 = Array<String>()
var array7:[String] = []
var array8:Array<String> = []

//define an array with same value
var array9 = Array<Int> (count: 10, repeatedValue: 1)

//拼接
var array10 = [12,13,14]
var array11 = array9 + array10
```

+ 数组基本操作

```
var array = ["A", "B", "C", "D", "E"]

array.append("F")
array += ["G", "H"]

array.insert("hello", atIndex: 0)
array.removeAtIndex(3)
array.removeLast()

array[1] = "KO"
array[2...5] = ["new"]

for index in 0..<array.count
{
    println(array[index])
}

for item in array
{
    println(item)
}

for (index, item) in enumerate(array)
{
    println("\(index)-\(item)")
}
```

+ swif中字典和OC中字典的区别（和数组差不多）

	- 所存储的的数据中，键和值可以是任意类型数据
	
	- 一个字典只能存储一种键和值的固定搭配
	
+ 字典的初始化

```
var dic1 = [22:"Hello", 44:"Jason", 33:"World"]
var dic2 = ["王府井":"商业中心", "天安门":"广场", "北京站":"火车站"]

var dic3:Dictionary<Int, String> = [12:"网站", 34:"Let's go", 33:"Who are you"]

var dic4:[Int: String] = [12:"网站", 34:"Let's go", 33:"Who are you"]  //注意用方括号声明时需要使用冒号

//define an empty dictionary
var dic5 = Dictionary<Int, String>()
var dic6 = [Int: String]()
var dic7:Dictionary<Int, String> = [:]  //方括号中要有冒号哦
var dic8:[Int: String] = [:]
```

+ 字典的基本操作

```
var dic1 = [22:"Hello", 44:"Jason", 33:"World"]

dic1.count
dic1.isEmpty

dic1[22]! //get value, it is Optional type
dic1[12] = "New" //add key/value
dic1[12] = nil   //del key/value
dic1[12]  //没有访问越界的限制

dic1.removeValueForKey(22)  //return the old value
dic1.updateValue("Kate", forKey: 44)  //replace, and return the old value

//遍历
for (key, value) in dic1
{
    println("\(key)-\(value)")
}

dic1.keys  //it likes an array, and we can use Array( dic1.keys ) to translate a real array
dic1.values  //like blove up

for key in dic1.keys
{
    println(key)
}

for value in dic1.values
{
    println(value)
}
```

+ switch控制，默认case中自带break属性，但也可以自行添加break，当想让其进入下一个case时可以使用fallthrough，但是当下个case中包含声明的变量时，会报错，也就是说使用fallthrough进入的下个case不能声明变量来接收判断值，如下

```
var a = 12
switch a
{
case 1...4:
    println("hello")
    fallthrough
case 5...12:
    println("world")
    fallthrough     //error, can't use fallthrough
case let x:
    println("\(x)")
default:
    break
}
```

+ break和continue，除了可以跳出本层循环外，还能通过标签跳出指定的循环

```
var buffer = Array< Array<Int>>()

for i in 0..<10
{
    buffer.append( Array<Int>(count: 10, repeatedValue: 0))
}

buffer[3][4] = 1

var k=0
mainLoop:
    for i in 0..<10
{
    for j in 0..<10
    {
        k++
        if buffer[i][j] == 1
        {
            break mainLoop
        }
    }
}
```

+ 函数内外变量名

```
//只拥有内部变量名
func sayHello(name: String, greeting: String) -> String
{
    return greeting + ", " + name + "!"
}

//拥有外部变量名
func sayHello2(Name name: String, Greet greeting: String) -> String
{
    return greeting + ", " + name + "!"
}

//内部变量名和外部变量名相同
func sayHello3(#name: String, #greeting: String) -> String
{
    return greeting + ", " + name + "!"
}

println(sayHello("Jason", "Hello"))
println( sayHello2(Name: "Jane", Greet: "Hi") )
println( sayHello3(name: "Kate", greeting: "Nice to meet you"))
```

+ 在函数的参数和返回值中使用可选性（Optional）和nil聚合功能，可以提高函数的健壮性

+ 参数的默认值以及传参顺序

```
//参数默认值,当参数有默认值时，参数的内部名自动提升为外部名，但仍可以自定义外部名或者使用＃
func sayHello4(name: String, greeting: String = "Hello") -> String
{
    return greeting + ", " + name + "!"
}

sayHello4("Jake")
sayHello4("Jake", greeting: "Hi")

//带有默认值的参数，在实际调用时可以不按原有顺序传参，但是没有默认参数的参数必须按照原有顺序传参
func sayHello5(name: String, greeting: String = "Hello", others: String = "How are you") -> String
{
    return greeting + ", " + name + "! and " + others
}

sayHello5("Lazzy", others: "Cherry", greeting: "Hey")
```

+ 可变参数和c中一样，在参数后面跟 ... 即可

```
func add(number:Int ...) ->Int?
{
    var result = 0
    
    if number.isEmpty
    {
        return nil
    }
    
    for a in number[0..<number.count]
    {
        result += a
    }
    
    return result
}

add(10, 1, 2, 5, 2)
```

+ 常量参数／变量参数／inout参数

	- 常量参数就是默认定义的参数类型，常量参数不能修改值
	
	- 变量类型要显示的使用var来定义参数
	
	- inout参数，以引用的方式将值传入参数，修改此值后会对外界产生影响
	
```
//let args
func myswap(a:Int, b:Int)
{
    var temp = a
    a = b  //error
    b = temp  //error
}

//var args
func myswap2(var a:Int, var b:Int)
{
    var temp = a
    a = b  //OK, but don't change the outside value
    b = temp
}

//inout args
func myswap3(inout a:Int, inout b:Int)
{
    var temp = a
    a = b  //OK, and now we can change the outside value
    b = temp
}

var a = 10,b=13
myswap3(&a, &b)  //使用时要加上&
```

+  函数类型的变量，函数类型的变量主要用于将函数作为参数传递给另一个函数，有点像c中的回调函数

```
func myadd(a:Int, b:Int) -> String
{
    return String(a + b)
}

//定义一个函数参数
let addFunc = myadd  //隐式声明
let addFunc2:(Int, Int) -> String = myadd //显示声明

//定义一个带有函数参数的函数，作为参数的函数应该不能拥有外部参数名，否则无法调用了
func showResult(#keyWord: String, #age:Int, #anotherFunc:(Int, Int) -> String) ->String
{
    return keyWord + anotherFunc(age, age+10) + "!"
}

showResult(keyWord: "Hello", age: 12, anotherFunc: myadd)  //Hello34!
```

+ 还能用函数类型的变量作为返回值

```
//返回值为另一个函数类型的变量
func add(a:Int, b:Int) -> (Int)->String
{

}
```

+ 一个函数还可以嵌套在另一个函数里面

```
func showName(name:String) ->String
{
    func greetName(greet:String, name:String)->String
    {
        return greet + ", " + name
    }
    
    return greetName("Hello", name)
}

showName("Jason")  //"Hello, Jason"
```

+ 闭包的写法和简化

```
var arr:[Int] = [8, 9, 4, 10, 34, 5, 1, 44, 33]

//标准类型必包，如果是闭包中有多行，则需要分行来写
var arr2:[Int] = arr
arr2 = sorted(arr2, {(a:Int, b:Int)->Bool in
        return a > b
})

//简化版1，如果只有返回值一句话，可以放倒一行上去，并且可以忽略return关键字
arr2 = arr
arr2 = sorted(arr2, {(a:Int, b:Int)->Bool in  a > b})
arr2

//简化版2，可以只写上参数名
arr2 = arr
arr2 = sorted(arr2, {a,b in a > b})
arr2

//简化版3，可以使用默认参数名，这时候in关键字需要被省略掉
arr2 = arr
arr2 = sorted(arr2, {$0 > $1})  //用$去取参数，和shell脚本一样
arr2

//简化版4，这种写法已经不是必包了，而是第二个参数就是>符号，其实这个符号也是一个函数，是比较函数
arr2 = arr
arr2 = sorted(arr2, >)
arr2
```

+ 闭包其实就是写在函数参数位置的代码块，是匿名的函数，这样写的方便之处是不需要去特意写一个函数，当然，如果是一个会被经常调用的代码块，还是封装成函数比较合适

+ 闭包另外两个特性
	- 当函数参数中最好一个参数为函数参数时，可以在此函数结束后紧跟一个闭包，即尾部闭包
	- 闭包中可以使用此闭包外面一层的变量，即外部值捕捉
	
```
//trailing closure
arr2 = arr
arr2 = sorted(arr2){(a:Int, b:Int)->Bool in
    return a>b
}
arr2

//cupture values
arr2 = arr
var num = 5
arr2 = sorted(arr2, {(a:Int, b:Int)->Bool in
    if a>num && b<num
    {
        return a>b
    }
    else
    {
        return a<b
    }
})
arr2
```

+ 值类型与引用类型
	- 各种基础变量类型都是值类型
	
	```	
	var int:Int = 1
	var float:Float = 1.0
	var double:Double = 1.0
	var bool:Bool = true
	var character:Character = "a"
	var tuble:(Int, String) = (2, "Hello")
	var string:String = "Hello World"
	var array:Array<Int> = [10,9,8,7,6]
	var dictionary:Dictionary<Int, String> = [1:"hello", 3:"Jason", 99:"99"]
	```
	- 函数和闭包是引用类型

+ enum  枚举

```
enum SomeEnumerate{
	case enumerate1
	case enumerate2
	...
}	
```

+ swift中的枚举和c／c++中有所不同，在这里是声明一个新的类型，所以首字母应该大写，其次，枚举中的成员不代表一个整型，也就无法和整形数进行比较。

```
enum Result
{
    case WIN
    case LOSE
    case DRAW
}

var yourScore = 100
var enemyScore = 90

var result:Result = Result.DRAW
if yourScore == enemyScore
{
    result = Result.DRAW
}
else if yourScore < enemyScore
{
    result = .LOSE
}
else if yourScore > enemyScore
{
    result = .WIN
}

switch result
{
    case .DRAW: println("You draw")
    case .WIN: println("You win")
    case .LOSE: println("You lsoe")
}
```
+ 枚举还能显式的关联一个类型

```
enum Month:Int
{
    case Jan = 1,Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec
}

enum Vowel:String
{
    case A = "a"
    case E = "e"
    case I = "i"
    case O = "o"
    case U = "u"
}

var a:Month = Month.Jan
a.rawValue  //return the real value, such as 1

var b:Vowel = Vowel.A
b.rawValue  //"a"

//use rawValue to create an Optional var or let
var c:Month? = Month(rawValue: 6)
c!.rawValue
```

+ 枚举还能为每个成员提供一个类型（用tuple的方式进行声明）

```
enum ProductCode
{
    case UPCA(Int, Int, Int, Int)  //uset Tuple to define
    case QRCode(String)
}

var productA = ProductCode.UPCA(2, 13, 56, 77)
var productB:ProductCode = .QRCode("Hi, I am the QRCode")

switch productA
{
case .UPCA(let a, let b, let c, let d):  //receive the the real value
    println("\(a) \(b) \(c) \(d)")
case .QRCode(let a):
    println(a)
}
```

+ 三种集合

	- 数组（Array），有序的数据集合
	- 字典（Dictionary），无序的键－值数据对集合	
	- 集合（Set），无序的数据集合

















