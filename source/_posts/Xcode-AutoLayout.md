title: Xcode Auto Layout
toc: true
date: 2015-09-03 16:15:37
tags: [Xcode, iOS]
categories: iOS 100 Days
description: 解释Xcode6中Auto Layout功能的使用
feature: http://7xj4cp.com1.z0.glb.clouddn.com/Xcode_icon.png
---

### 他是什么

Auto Layout功能是Apple推出的用于适应各个不同尺寸屏幕设备的自动布局功能。

### 怎么用

+ 首先给这个controller选中“use auto layout”选项

<!-- more -->

![use auto layout](http://7xj4cp.com1.z0.glb.clouddn.com/use_auto_layout.png)

+ 然后在每个页面控件布局结束后，选择底部“reslove auto layout issues”->"Add missing constraints"，这样就会给这个页面中的每个控件添加相应的constraints

![reslove auto layout issues](http://7xj4cp.com1.z0.glb.clouddn.com/resolve_constraints.png)

+ 这里的constraints可以针对某个控件进行操作，也可以针对一个页面中的所有控件进行操作

+ 如果添加了constraints可以在右侧的“size inspector”上看到相应的constraint，可以对其进行编辑

![constraints editor](http://7xj4cp.com1.z0.glb.clouddn.com/constaints_editor.png)

+ 也可以通过右键控件，然后在页面中滑动鼠标的方式添加constraints

### reslove auto layout issues

+ "update frames"是对移动后的控件进行复位的

+ "update constraints"是对移动后的控件进行实际的位置改变，否则会在页面中显示黄线，用以提示开发者控件只是移动了，但并未落实到实际的constraint上。

+ "Add missing constraints"对控件添加默认的constraints

+ "reset to suggested constraints"将控件设置到建议的constraints上

+ "clear constraints"清除控件或者页面所有的constraints

### 如何验证

做了自动化布局后最好进行验证，以确保真正是在所有尺寸的设备上都能正常显示控件。

使用“Assistant Editor”，然后调用“Preview”就能显示出在各种尺寸设备上的显示状况了。（在左下角可以添加多个尺寸的预览）

![Assistant Editor](http://7xj4cp.com1.z0.glb.clouddn.com/preview.png)

![many inch](http://7xj4cp.com1.z0.glb.clouddn.com/many_inch.png)

