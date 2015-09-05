title: goldenDict
toc: true
date: 2015-09-02 18:12:25
tags: [English]
categories: Tools
description: goldenDict introduction
feature: http://7xj4cp.com1.z0.glb.clouddn.com/programicon.png
---

### 他是什么
goldenDict 是由一位俄国程序员开发的源代码开放的词典工具，可以加载多种不同格式的词典包，不仅能显示词语解释，还能提供相关的语音功能和屏幕取词功能。

下面是这个软件的网页：http://goldendict.org

官网提供win32和win64版本的安装程序，并且提供源码下载。

### 为什么是他
很多朋友会使用有道词典或者金山词霸等词典，这些词典也不错，但也存在一些问题。

比如有道的语音包都是电脑合成语音（这也就是为什么有道提供的语音包很小的原因），有些词的发音和地道的美音英音会有差别，同时这些发音的音量也很让人诟病，有些词的发音音量很小以至于需要调高系统音量，而有些词的发音音量有很大，这让用户的体验相当的差。由于学习原因，本人需要经常查单词，有道词典的这个发音问题真的让我很抓狂。
<!-- more -->
再比如，例句少，解释简单，没有更深入的讲解。

再比如，让用户体验更加糟糕的广告以及反应速度。

而开源的字典工具就很少会有上面的这些问题，而开源字典中的翘楚则非goldenDict莫属了（starDict也很优秀，但相对goldenDict而言支持的字典格式少了点）

goldenDict支持多种格式的词典，包括Babylon 的 .BGL 文件、StarDict 的 .ifo/.dict/.idx/.syn 文件、Dictd 的·index/.dict(.dz) 文件、ABBYY Lingvo 的 .dsl/.lsa/.dat 文件等等，每一种格式的词典背后都用强大的社区在为其提供帮助，这些格式的词典有一些会提供语音包，下载到本地后就可以直接使用，不需要像有道词典一样在线获取语音。

例句少，解释简单？由于支持多种格式的词典，所以会有更多的例句提供和更深入的讲解，并且有些单词会配有图片。

广告？这是开源的软件，没有盈利性质，所以不会有广告的介入。

反应速度？在我的电脑少反应速度还是很不错的，未出现过类似有道词典突然崩溃的问题

### 如何获取

由于goldenDict只是词典工具，所以你还要下载某些词典包才能真正的使用，下面会介绍词典工具和词典包的相关下载。

goldenDIct安装文件下载：

[官网下载](http://goldendict.org/download.php)  **官网只有windows版本的**

[win百度网盘下载](http://pan.baidu.com/s/1jGEQabG)  **这个是windows版本的**

[Mac百度网盘下载](http://pan.baidu.com/s/1kT0AOUb)  **这个是本人自己编译的Mac版本**

goldenDict源文件下载：

[官网下载](http://goldendict.org/download.php)

[百度网盘下载](http://pan.baidu.com/s/1pJOjoa3)

本人所使用的词典包：

[百度网盘下载](http://pan.baidu.com/s/1c0FImQk)  **有8个词典包，部分词典包包含语音，总共3.15G**

如果你还想要其他的词典包可以自行百度搜索。

#### 8个词典包的简介
毕竟8个包有3.15G，在这里还是简介一下好，免得有人下载了自己其实并不需要的包。

##### En-En_Longman_DOCE5

《朗文当代英语辞典》（Longman Dictionary of Contemporary English）5th Edition

这个词典包是带语音的，例句丰富并且例句也提供语音，非常适合英语学习者的词典包

同时，这个词典包文件夹中还额外拥有一个扩展词典包，用于提供大量的常用词组和例句，但是这部分的例句没有语音服务

##### En-En_Longman_Pronunciation3

《朗文英语发音词典》(Longman Pronunciation Dictionary ) 3rd Edition

这个词典包带有语音，但不包含单词解释，只有一些单词的常用短语的发音，算是一个扩展词典

##### En-En_Merriam_Webster11

《韦氏词典》，他是美国的非常实用的权威字典

这个词典包也带有语音服务，但只对所查的单词提供语音，对例句则没有语音。

里面的解释很丰富，但例句较少

##### En-En_OALD8

《牛津高阶》(Oxford Advanced Learner's Dictionary,OALD)8th Edition，他是中国乃至全球最畅销的英语学习词典

这个词典包带有语音，但同《韦氏词典》一样，只对所查单词提供语音

例句很多，讲解也很深入

##### En-zh_CN_OALD4

前面几个词典包都是英英，也就是用英语去解释英语，接下来几个则是英汉的。

《牛津高阶》第四版，英汉的，没有语音

##### En-zh_Collins

《柯林斯》英汉词典，没有语音

##### En-zh_langdao

《朗道》英汉词典，没有语音，例句极少

##### zh-En_langdao

《朗道》汉英词典，没有语音

### 如何使用

当你把词典工具和词典包都下载到本地后，你需要在词典工具中添加相应的词典包。

	Edit->Dictionaries
	
然后点击添加即可，添加时添加相应词典包的文件夹即可。

![添加词典包](http://7xj4cp.com1.z0.glb.clouddn.com/add_dictionary.png)

##### 添加构词法

在查询单词时我们会发现，如果查boxes是查不出正确的单词box的，最多会有一些正确单词的提示，这是因为我们未关联英文的构词法。

![搜索](http://7xj4cp.com1.z0.glb.clouddn.com/search_boxes.png)

windows版本的goldenDict安装完之后便会有构词法，只是默认是未关联的，我们需要手动进行关联。

而Mac版本的goldenDict默认是没有构词法的，我们需要像添加词典包一样添加构词法，然后进行关联。

[构词法百度网盘下载](http://pan.baidu.com/s/1o6DvXYq)

![未添加构词法](http://7xj4cp.com1.z0.glb.clouddn.com/add_morphology.png)

![添加构词法](http://7xj4cp.com1.z0.glb.clouddn.com/add_morphology2.png)

---

添加完词典包和构词法就能使用了，使用来goldenDict来工作和学习吧！

*原著博客，转载请注明*
