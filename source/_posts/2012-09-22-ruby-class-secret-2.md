---
title: "Ruby Class Secret(Part II)"
date: 2012-09-22 23:30:00
tags: [ruby]
---

让我们继续探索Ruby的Class

##类定义

静态语言中，类的定义给人的感觉更多是在定义一种数据结构，及该结构上的一系列行为，只有实例化一个对象的时候，其代码才真正的生效或者被执行。而Ruby则不同，`class`关键字之后，实际就是在执行一段普通的代码，举个例子
{% codeblock lang:ruby %}
class MyClass
  puts 'Hello!'	# Hello!
end
{% endcodeblock %}
运行该脚本，除了定义`MyClass`类之外，还会直接在控制台输出字符串。

##类实例变量&类方法

先看一段代码
{% codeblock lang:ruby %}
class MyClass
  @my_var = 1

  def self.read
    @my_var
  end
end
{% endcodeblock %}
大家一定要注意`@my_var`可不是这个类的对象的实例变量，而是`MyClass`类的实例变量。实例变量的归属取决于上下文，在`class`关键字后，`self`所代表的当前对象是`MyClass`，**类也是对象**。也许你会觉得类实例变量类似于Java中的静态成员，但事实并非如此，类实例变量与对象实例变量处于完全不同的两个上下文，普通的对象无法访问类实例变量，但Java中类的对象可以访问类的静态成员（Ruby中类似的概念应该是`@@`开头的变量，但这种变量不推荐使用）。再进一步，类实例变量完全是该类独有的，即使是该类的子类也看不到这个变量。

`MyClass`中定义的`read`方法也不是普通的实例方法，而是类方法，由于类方法的方法体内处于类的上下文中，可以访问到类实例变量。

##单件方法

Ruby允许你为某个具体对象添加一个方法，这种方法即为单件方法，定义单件方法的方式如下
{% codeblock lang:ruby %}
str = "just a regular string"

def str.title?
  self.upcase == self
end

str.title?	# false
{% endcodeblock %}
其实上节中的类方法也是一个单件方法，与上面的例子稍有不同的是，该方法添加在了一个类上，这里又要强调了，**类也是对象**，因此本质是完全一样的。类方法的一个应用就是在`Module`中定义的访问器（attr_accessor等）。

##终极藏宝图

引入了单件方法的概念后，我们有必要重新审视一下类与对象的关系图，单件方法应该存在于哪里呢
![class_and_object_3](/images/method_finding.png)

看来类的真相[上篇](/2012/09/06/ruby-class-secret-1/ "Ruby类的真相(上)")。中获得的藏宝图并不全，Ruby刻意隐藏了一些内幕，让我们发掘发掘，单件方法在哪里呢？答案是eigenclass。每一个对象对应一个特有的隐藏类，这个类就是eigenclass，该类只有一个实例，且不能被继承，单件方法就存在于eigenclass中，后续表述中将一个对象`obj`对应eigenclass记为`#obj`。以下面的代码为例
{% codeblock lang:ruby %}
class C
  def a_method
    'C#a_method'
  end

  def self.a_class_method
    'C.a_class_method'
  end
end

class D < C; end

obj = D.new

def obj.a_singleton_method
  'obj#a_singleton_method'
end
{% endcodeblock %}
当方法查找过程向一个对象索要类时，你首先得到的是该对象的eigenclass，如果eigenclass中没有要调用的方法才会继续沿着原有祖先链向上查找
![class_and_object_4](/images/method_finding_with_eigenclass.png)

调用一个类的类方法时，方法查找的过程也类似，首先会向右一步进入该类的eigenclass，再向上，这里还有一个知识点就是**eigenclass的超类就是超类的eigenclass**，完整藏宝图如下
![class_and_object_5](/images/everything.png)

##心法口诀

总结一下类的真相，共有7招：  
1.  只有一种对象——要么是普通对象，要么是模块、类；        
2.  只有一种模块——可以是普通模块、类、eigenclass；           
3.  只有一个方法，它存在于一种模块中——通常是类中；          
4.  每个对象（包括类）都有自己的“真正的类”——要么是普通类，要么是eigenclass；        
5.  除了BasicObject类无超类外，每个类有且只有一个超类。这意味着从任何类只有一条向上直到BasicObject的祖先链；      
6.  一个对象的eigenclass的超类是这个对象的类；一个类的eigenclass的超类是这个类的超类的eigenclass；        
7.  当调用一个方法，Ruby先向“右”迈一步进入接收者真正的类，然后向“上”进入祖先链。
