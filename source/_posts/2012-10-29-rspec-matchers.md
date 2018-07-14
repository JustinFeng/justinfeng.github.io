---
title: "RSpec Matchers"
date: 2012-10-29 21:09:00
tags: [ruby, rspec]
---

RSpec是Ruby世界所熟知的测试框架，它可以用于**单元测试**及**功能测试**（与Cucumber配合）。与其他测试框架一样，完成测试功能、实现红绿循环的核心是断言语句，而RSpec的断言是通过`{}.should`、`expect{...}.to`+Matcher实现的。下面就总结一下RSpec内建的Matcher以及如何定制自己的Matcher。

##内建Matcher

###include

数组中是否包含某元素
{% codeblock lang:ruby %}
milan_players.should include('Maldini')
{% endcodeblock %}

###respond_to

对象是否有某个方法
{% codeblock lang:ruby %}
'Milan'.should respond_to(:length)
{% endcodeblock %}

###raise_error, throw_symbol

代码执行时是否抛出异常，如果不传参数则验证任意异常的抛出；如果给一个参数，可以是某个具体的异常类或者异常描述（支持正则匹配）；当然也可以给两个参数，第一个为异常类，第二个为异常描述
{% codeblock lang:ruby %}
lambda { Object.new.explode! }.should raise_error(NameError)
{% endcodeblock %}

###==, ===, equal, eql

上述几个Matcher用于测试相等，其效果与Ruby中对应的操作符或方法相同
{% codeblock lang:ruby %}
(3 * 5).should == 15
{% endcodeblock %}

注意：RSpec不建议使用`!=`，可以用`should_not ==`代替

###be_close

可以用来测试浮点型数据是否相差在某个误差内
{% codeblock lang:ruby %}
result.should be_close(5.25, 0.005)
{% endcodeblock %}

###match, =~

验证正则表达式匹配
{% codeblock lang:ruby %}
	result.should =~ /this expression/
{% endcodeblock %}

###change

判断数值变化，相关使用方法非常接近自然语言，不多做解释，看例子
{% codeblock lang:ruby %}
expect {
  User.create!(:role => "admin")
}.to change{ User.admins.count }

expect {
  User.create!(:role => "admin")
}.to change{ User.admins.count }.by(1)

expect {
  User.create!(:role => "admin")
}.to change{ User.admins.count }.to(1)

expect {
  User.create!(:role => "admin")
}.to change{ User.admins.count }.from(0).to(1)
{% endcodeblock %}

###布尔Matcher

当一个对象中有布尔方法`XXX?`，则RSpec会自动为其生成如下的几个Matcher
	
	be_XXX
	be_a_XXX
	be_an_XXX


###be_true(false)

用来断言真假时，还可以直接使用如下的Matcher

	be_true
	be_false

###have开头的布尔Matcher

当一个对象中有`has_XXX?`方法时，RSpec会为其自动生成`have_XXX`的Matcher
{% codeblock lang:ruby %}
request_parameters.should have_key(:id)
{% endcodeblock %}

补充：为了使断言语句更加接近自然语言，如果在Matcher上调用不存在的方法，该方法会被代理到Matcher所传给的目标对象，漂亮的语法糖
{% codeblock lang:ruby %}
home_team.should have(9).players_on(field)
{% endcodeblock %}

###集合Matcher

对于集合对象来说，还有几个内建的Matcher非常直观和实用
{% codeblock lang:ruby %}
day.should have_exactly(24).hours
dozen_bagels.should have_at_least(12).bagels
internet.should have_at_most(2037).killer_social_networking_apps
{% endcodeblock %}

###运算符Matcher

处理前面提到的一些运算符Matcher之外，常用的还有
{% codeblock lang:ruby %}
result.should be < 7
result.should be <= 7
result.should be >= 7
result.should be > 7
{% endcodeblock %}

##定制Matcher

如果RSpec内建提供的Matcher不能满足需求，你还可以自己定制专用的Matcher，具体的定义方法如下
{% codeblock lang:ruby %}
RSpec::Matcher.define :report_to do |boss|
  match do |employee|
    employee.reports_to?(boss)
  end

  failure_message_for_should do |employee|
    "expected the team run by #{boss}" to include #{employee}"
  end

  failure_message_for_should_not do |employee|
    "expected the team run by #{boss}" to exclude #{employee}"
  end

  description do
    "expected a member of the team run by #{boss}"
  end
end
{% endcodeblock %}

上面代码中给出了定义一个Matcher较为完整的方式，定义中的4个块传递并不都是必须的，要完成一个Matcher的定义必须提供的内容为`match`块和`failure_message_for_should`块，另外两个块是可选的。

掌握了自定义Matcher的方法，相信从技术上应该没有什么断言语句是你写不出来的喽：）    
