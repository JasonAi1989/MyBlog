title: markdown流程图语法 
toc: true
date: 2015-03-17 21:26
tags: [markdown]
categories: markdown
description:
feature:
---

从网上找了很久关于markdown语法的文章，机会微乎其微，大多所指向的都是同一个页面https://github.com/adrai/flowchart.js
这是github上的一个开源项目，里面对我有用的只有一小段文字

<!-- more -->

```
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: My Operation|past
op2=>operation: Stuff|current
sub1=>subroutine: My Subroutine|invalid
cond=>condition: Yes 
or No?|approved:>http://www.baidu.com
c2=>condition: Good idea|rejected
io=>inputoutput: catch something...|request

st->op1(right)->cond
cond(yes, right)->c2
cond(no)->sub1(left)->op1
c2(yes)->io->e
c2(no)->op2->e
```



由于墙的原因，也没法找到什么有用的外文资料，只能总结一下此段代码里面的语法了。
流程图的语法大体分为两段，第一段用来定义元素，第二段用来连接元素
定义元素阶段的语法是
tag=>type: content:>url
tag就是一个标签，在第二段连接元素时用
type是这个标签的类型，从上段内容看有6中类型，非别为：

```
start
end
operation
subroutine
condition
inputoutput
```
content就是在框框中要写的内容，中英文均可，但有一点需要特别注意，就是type后的冒号与文本之间一定要有个空格，没空格会出问题。
url就是一个连接，与框框中的文本相绑定

连接元素阶段的语法就简单多了，直接用->来连接两个元素，需要注意的是condition类型，因为他有yes和no两个分支，所以要写成

```
c2(yes)->io->e
c2(no)->op2->e
```

这样的格式。
下面为显示情况
StartMy OperationYes or No?Good ideacatch something...End StuffMy Subroutineyesnoyesno

如果IE浏览器显示不好，可以使用chrome试试。