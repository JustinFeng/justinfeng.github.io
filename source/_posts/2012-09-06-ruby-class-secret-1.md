---
title: "Ruby Class Secret(Part I)"
date: 2012-09-06 23:25:00
tags: [ruby]
---

提起Ruby，大家更多会联想到Rails，正是Rails的蓬勃发展，使得Ruby为更多的Coder所熟悉。但换位思考一下，如果不是Ruby语言特性的灵活，又怎么会有Rails的敏捷。因此在这里想跟大家分享一点Ruby语言的**元编程**特性，特别是Ruby类背后的故事。

##打开类

关于打开类，先看一个简单的例子
{% codeblock lang:ruby %}
class Fijo
  def x; 'x'; end
end

class Fijo
  def y; 'y'; end
end

obj = Fijo.new
obj.x 	# 'x'
obj.y 	# 'y'
{% endcodeblock %}

上面是一个最简单的打开类的实例，你可以将第一个`class`认为是定义一个不存在的类，但怎么理解第二个`class`的工作方式呢？

从某种意义上来说Ruby的`class`关键字更像是一个作用域操作符，而不是一个类型声明语句。它的确可以创建一个不存在的类，但对于`class`关键字，其核心任务是将你带到**类的上下文**中，让你可以修改类的行为。

如果你的工作中仅使用一些静态语言，如Java, C++等，打开类这样的特性一定让你感到耳目一新。但我不得不将你拉回现实，事物都有两面性，打开类也会引发严重的问题，**猴子补丁**就是其中之一，有兴趣的童鞋可以Google之。

##对象中有什么

先来一段code
{% codeblock lang:ruby %}
class MyClass
  def my_method
    @v=1
  end
end

obj=MyClass.new
obj.my_method
{% endcodeblock %}
与其他静态语言不同，Ruby中类的实例并不会在初始化时获得所有的实例变量，而仅在对某实例变量赋值后才会生成，以上面的代码为例，`obj`在被初始化时还没有实例变量`@v`，但执行`my_method`后，`@v`就生成了。

而实例方法则不同，它们存在与对象所属的类中。这里有一个概念，就是一个对象的方法称为其类的实例方法。相应的关系可以用下图来描述，后续关于Ruby类故事的主线就是对这个图的演进：
![class_and_object_1](/images/var_in_object.png)

##类也是对象

类也是对象，类的类是`Class`，因此所有适用于对象的的规则，也同样适用于类。既然一个对象的方法是其类的实例方法，这就意味着一个类的方法是`Class`的实例方法（至少目前可以仅仅这样理解:)）。

另外，虽然可能会让你有些混乱，但还是要将类关系补充完整，在继承关系中，`Class`的父类是`Module`，`Module`的父类是`Object`，`Object`的类是`Class`，`Class`的类是其自身。
![class_and_object_2](/images/class_is_object.png)

##方法查找

当一个对象调用一个方法时，首先需要做的就是方法查找，为了了解方法查找的过程，需要先了解两个概念：**接收者**和**祖先链**。**接收者**就是调用方法的那个对象；**祖先链**则是从对象的类到`Object`的类路径。

方法查找的过程就是首先在对象所在类中查找被调用的方法，如果无法找到则在一层层的祖先链中查找，如果直到`Object`都无法找到对应的方法，则会调用神奇的`method_missing`方法。这个方法查找的过程可以总结为“向右一步，再向上”。
![class_and_object_3](/images/method_finding.png)

这里需要额外说明一点，这里的祖先链也包括模块，一个类所包含的模块会被插入到祖先链中该类与超类（或模块）之间。

---

到这里已经介绍了对于Ruby类与对象的基本知识，但进一步的内容还请移步[下篇](/2012/09/22/ruby-class-secret-2/ "Ruby类的真相(下)")。
