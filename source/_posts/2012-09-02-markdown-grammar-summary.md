---
title: "Markdown Grammar Summary"
date: 2012-09-02 20:40:00
tags: [markdown]
---

Markdown的最初目标是让网络上的阅读和写作都更加方便快捷。所有的Markdown符号都使用的标点符号，可以说它是一个专门用于网络写作的技术，这个博客就是使用Markdown来编写的。

Markdown主要涵盖了Html中文本相关的功能，如果希望在Markdown中使用其他更加绚丽的表现方式，最简单的方法就是直接内联嵌入Html代码。这里提醒一点，对于`<div>`、`<table>`、`<pre>`和`<p>`，必须在标签的前后保留空格，便于Markdown的解释器辨识，但这些标签中不能再嵌入Markdown代码。

##换行

在行末增加2个以上的空格可以实现换行

##标题

行首插入`#`来创建标题，`#`个数决定标题级别

	# This is H1
	## This is H2

##引用

在行首加入`>`来创建引用，引用中还可以创建其他Markdown标记，甚至是引用

	> Quote block

##列表

使用`*`创建无序列表，使用数字加英文句号`x.`创建有序列表，注意这里列表符号和文字内容之间要有2个空格

	*  ul
	1.  ol

##代码

使用tab创建一个代码片段（No example）

##行内代码

使用撇号`` ` ``包围内容创建行内的代码块

	Try this `code` on your console

##分割线

使用3个或以上的减号`---`创建一个分割线

	---

##链接

方括号内是链接文字，小括号内是url，双引号内是链接的提示

	[JustinFeng](http://justinfeng.github.io/ "Blog")

##直接链接

使用尖括号包围url创建直接链接

	<http://justinfeng.github.io>

##强调

单`*`包围实现强调，双`**`包围实现加粗

	*emphasis*
	**strong**

##图片

使用下面格式创建一个图片

	![Alt text](/path/to/img.jpg)

##转义

在使用Markdown时如果需要使用特殊符号，同样可以使用`\`进行转义处理

##写在最后

本文以简洁的方式列出了Markdown中最常用的标记和语法，目的是让读者**快速上手**，或者进行**速查**。完整内容请深入学习[Markdown](http://daringfireball.net/projects/markdown/syntax "Markdown")
