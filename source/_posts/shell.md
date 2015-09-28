title: shell笔记
toc: true
date: 2015-04-4 14:46:42
tags: [shell]
categories: shell
description:
---

## 判断 -- if
#### 语法
    if [  ]
    then
        command
    else
        command
    fi
或者

    if [  ]；then
        command
    else
        command
    fi

<!--more-->
#### 判断变量是否等于某个值
    c=1
    if [ $c -eq 4 ] 或者 if [ $c = 4 ]
    then
        echo yes
    fi

#### 判断变量是否等于某个字符串
    a="abc"
    if [ "$a" = "abc" ] 
    then
        echo "equal"
    fi
***注：此处不能用if [ "$a" -eq "abc" ]会报错***

#### 判断字符串、输入参数是否为空
    if [ ! -n "$1" ]
    then
        echo "you don't have input arg"
        exit
    fi

#### 判断文件夹是否为空、是否存在
    dir=/home
    if [ ! -d "$dir" ]
    then
        echo "$dir is not a directory"
        exit
    fi

#### 判断两个字符串是否相等
    if [ "$file_1" != "$file" ]
    then
        file=$file_1
    else
        file=$file_2
    fi

## 字符串操作
例：

    string="123!@#$%^&*()_+"
    position=3
    length=4
    
```
${#string} $string的长度
    ${#string}  # 15
    
${string:position} 在$string中，从位置$position开始提取子串,第一个字符位置为0
    ${string:position} # !@#$%^&*()_+
  
${string:position:length}  在$string中, 从位置$position开始提取长度为$length的子串  
    ${string:position:length}  # !@#$
    
${string#*substring}  从左向右，截取第一个substring之后的内容，此处substring不是变量而是具体的字符串
    ${string#*@#}   # $%^&*()_+

${string##*substring}   从左向右，截取最后一个substring之后的内容，此处substring不是变量而是具体的字符串
    ${string##*@#}  # $%^&*()_+
    
${string%substring*}  从右向左，截取第一个substring之后的内容
    ${string%@#*}    # 123!

${string%%substring*}  从右向左，截取最后一个substring之后的内容
    ${string%%@#*}   # 123!
    
${string/substring/replacement}  使用replacement来代替第一个匹配的substring，这里也是用字符串而不是变量
    ${string/@#/ER}  # 123!ER$%^&*()_+

${string//substring/replacement}  使用replacement代替所有匹配的substring                 
    ${string//@#/ER} # 123!ER$%^&*()_+

```

## 参数
$0 至 $9代表参数
$@ 代表所有参数

## 取变量、执行命令、操作
$a  取变量a的值

$(pwd)  把（）内的内容当作命令去执行

${ }  做某些操作，比如截取字符串会用到这个 

## 字符串和变量拼接
"hello $a" 或者 "hello "$a

## ps aux | grep app
让返回的结果只包含需要查询的结果，不包含grep app

    ps aux | grep \[a]pp    
   
牛逼的操作

## for循环
格式：

    for var in word1 word2 ....
    in
        command
        command
    done

   谈到循环，就不得不说$@变量，它代表该shell脚本的所有参数。所以，要写一个命令行中键入的所有参数的程序就应该向下面这样：
   
    for arg in "$@"
    do
        echo $arg
    done

说到$@就不得不说for循环的另一种形式，就是缺省参数
    
    for var
    do
        command
        command
    done
上面的程序等价于：

    for var in "$@"
    do
        command
        command
    done

下面的代码中，变量i在每次迭代的过程里都会保存一个字符，范围从a到z:
    
    for i in {a..z}
    do 
        actions
    done

for 循环也可以采用C语言中的for循环格式。例如：

    for (( i=0; i<10; i++))
    {
        commands;
    }

在此说一下常用的两个结构：
 
1.

    for i in $(seq 1 100)
    do 
        echo $i 
    done
    

2.

    for (( i = 1 ; $i <= 100; i++ ))
    do 
         echo $i; 
    done


## while循环

    while command1
    do
        command
        command
    done
    
## break和continue仍然可以使用

## 变量类型
不管是出现在函数里面还是外面的变量都是全局变量，只有被定义成local的变量才是局部变量   

    local a       
则a是局部变量，只能在作用域内有效，此时如果和全局变量重名，则在作用域内局部变量优先被使用

## shell脚本的续行符为   \

## case结构

    case  $val in
    a)
	    command;;
    c)  
	    command;;
    *)
	    command;;
    esac
    
## 将变量的内容追加到某个文件中

    echo "[$file]" | cat >> $log
    
## find
find发现的文件含有空格，可以在空格的前面加上\然后在后续进行解析，合成后面可用的字符串,像其他一些命令使用这种带空格的字符串时可以加上双引号，便可以正常使用

    find $instead_path -type f|sed -e 's/ /\\ /g'
    
## 转义字符  \

## 函数的返回值
	$?  可以获取上一个函数的返回值
	
## 从命令行获取内容
	read val
可以将内容读到val中，以回车为结束符

## 函数
 	funcname()
	{}
函数可以有返回值，函数内的局部变量用local定义，如果没有local则是全局变量
函数调用时
	
	funcname $file
直接加变量即可，不需要小括号

## if时多个判断条件
    if [ "${file:0-11}" = ".properties" ] || [ "${file:0-4}" = ".pdf" ] || [ "${file:0-5}" = ".html" ]
可以使用 || 和 &&

## 变量说明
    $$
Shell本身的PID（ProcessID）
    
    $!
Shell最后运行的后台Process的PID

    $?
最后运行的命令的结束代码（返回值）
    
    $-
使用Set命令设定的Flag一览
    
    $*
所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
    
    $@
所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
    
    $#
添加到Shell的参数个数

    $0
Shell本身的文件名

    $1～$n
添加到Shell的各参数值。$1是第1参数、$2是第2参数…。

## 变量自增的实现方法
	1. i=`expr $i + 1`;
	2. let i+=1;
	3. ((i++));
	4. i=$[$i+1];
	5. i=$(( $i + 1 ))
最简单的方式是使用let命令，let命令可以直接执行基本的算数操作，当使用let时，变量名之前不需要再添加$，例如

    arg1=10
    arg2=23
    let result=arg1+arg2
    echo $result
    
自加：      

    let i++
    
自减：
	 
    let i--
    
简写形式 

    let arg+=6
    
其他四种方法也可以做算数运算，但只支持整数运算，不支持浮点型

## 与文件存在与否的判断
    -e 是否存在
    -f 是否为普通文件
    -d 是否为目录
    -s 是否为空的文件
    -p 是否为管道文件
    -b 是否为块设备文件
    -c 是否为字符设备文件
    -L 是否为软链接
    -S 是否Socket文件
 
## 与文件权限有关的判断

    -r 是否有可读的权限
    -w 是否有可写的权限
    -x 是否有可执行权限
    -u 是否有特权位
    -g 是否有组特权位
    -k 是否有t位，即粘贴位

## 两个文件的比较判断

    -nt 比较file1比file2新
    -ot 比较file1比file2旧
    -ef 比较file1和file2是否为同一个文件，
    一般用于判断硬链接

## 整数之间的大小判断

    -eq 相等
    -ne 不等于
    -gt 大于
    -ge 大于等于
    -lt 小于
    -le 小于等于

## 字符串之间的判断

    -z 是否为空字符串
    -n 是否为非空字符串
    str1 = str2 是否相等
    str1 != str2 是否不等

## 多重条件判断

    -a 两个条件同时满足，就为真，相当于and
    -o 两个条件满足其一，就为真，相当于or

 如果使用 [[ ]], 则多重判断可以使用:
 
    [[ xxx && xxx || xxx ]] 
的形式.


## 获取某个进程的pid的方法
可以使用ps+grep，然后在进行解析的方式获取到；

也可以使用命令pgrep获取，这种方法很好用，但缺点是不能根据信息进行pid的查找，只能根据可执行文件的名字进行查找

在使用pgrep时最好加上-f选项，以便使用完整文件名进行匹配，-l会列出所有和程序名匹配的进程以及其进程名

## 对文件添加特殊保护
linux lsattr  chattr

#### chattr命令的用法：

chattr [ -RV ] [ -v version ] [ mode ] files...

最关键的是在[mode]部分，[mode]部分是由+-=和[ASacDdIijsTtu]这些字符组合的，这部分是用来控制文件的属性。

    + ：在原有参数设定基础上，追加参数。
    - ：在原有参数设定基础上，移除参数。
    = ：更新为指定参数设定。
    A：文件或目录的 atime (access time)不可被修改(modified), 可以有效预防例如手提电脑磁盘I/O错误的发生。
    S：硬盘I/O同步选项，功能类似sync。
    a：即append，设定该参数后，只能向文件中添加数据，而不能删除，多用于服务器日志文 件安全，只有root才能设定这个属性。
    c：即compresse，设定文件是否经压缩后再存储。读取时需要经过自动解压操作。
    d：即no dump，设定文件不能成为dump程序的备份目标。
    i：设定文件不能被删除、改名、设定链接关系，同时不能写入或新增内容。i参数对于文件 系统的安全设置有很大帮助。
    j：即journal，设定此参数使得当通过mount参数：data=ordered 或者 data=writeback 挂 载的文件系统，文件在写入时会先被记录(在journal中)。如果filesystem被设定参数为 data=journal，则该参数自动失效。
    s：保密性地删除文件或目录，即硬盘空间被全部收回。
    u：与s相反，当设定为u时，数据内容其实还存在磁盘中，可以用于undeletion.
各参数选项中常用到的是a和i。a选项强制只可添加不可删除，多用于日志系统的安全设定。而i是更为严格的安全设定，只有superuser (root) 或具有CAP_LINUX_IMMUTABLE处理能力（标识）的进程能够施加该选项。

freebsd ls -lo   chflags

    chattr +i = chflags schg
    chattr -i = chflags noschg
    

## 浮点数运算
bash无法进行浮点数运算，需要借助bc，awk

例：

    c=$(echo "5.01-4*2.0"|bc)
    echo $c
或者

    c=$(awk 'BEGIN{print 7.01*5-4.01 }')
    echo $c
    
## 在shell 中$() 与 ``等效。 中间包含命令语句执行，返回执行结果。


## 使用ps显示某个进程的父进程pid

    ps -axo pid,ppid,comm,pcpu,pmem
    
## 用awk打印某一列
    awk '{print $1}' 打印第一列
    awk '{print $1,$2}' 打印第一二列
    awk '{print $1"@#"$2}' 和字符串打印到一起

## find命令的参数
    -type:查找某一类型文档
    b:块设备文档
    d:目录
    c:字符设备文档
    P:管道文档
    l:符号链接文档
    f:普通文档
    
