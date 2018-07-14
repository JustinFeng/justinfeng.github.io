---
title: "Image Overlap in Email"
date: 2013-04-25 23:22:00
tags: [html, email]
---

Web前端需要使用到图片叠放的应用场景有很多，一个比较典型的情况是在Youtube视频的预览图片上添加一个Play按钮，上网搜索一下就会发现有很多方式可以将一个图片叠放到另一个图片上：  
1.  使用绝对定位，`position:absolute`；        
2.  将叠放下方的图作为背景图片；           
3.  使用负的margin将一个图片移动到另一个之上。    
4.  ……

##Email世界

当你需要在Email的内容中实现图片叠放时，我不得不遗憾的告诉你：上述方法均不可行，其主要原因是在Gmail, Hotmail，YahooMail等Web邮件客户端中，诸如position，background，margin等样式都是不被支持的。

如果你正在被上面提到的问题所困扰，不用着急，下面就来揭晓Email世界中的完美解决方案，不需要fancy的标签和css，你需要的只是平凡的table span。（推荐你在[JsBin](http://jsbin.com/)上面试试下面的Html代码以帮助理解。）
 
##独立图片

当你仅需要在一个独立的图片上叠加图片时，解决方法是相对简单的，可以利用rowspan和colspan来重合两个表格单元完成图片的叠放：
{% codeblock lang:html %}
<table>
  <tr>
    <td></td>
    <td rowspan=2>
      <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align: center">
      <img style="border: 1px solid;background-color: aquamarine; width:60px; height:45px"/>
    </td>
  </tr>
</table> 
{% endcodeblock %}

效果如下：
![email_image_1](/images/email_image_1.png)

##一组图片

当你需要给一组图片中的一张上叠加图片时，事情会变的稍微有些复杂，此时你可能发现使用上面的办法会破坏该图片与其他图片的对齐，大多数情况会有令人恼火的1px偏移，试试下面的代码：
{% codeblock lang:html %}
<table>
  <tr>
    <td>
      <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>  
    <td>
      <table>
        <tr>
          <td></td>
          <td rowspan=2>
            <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
          </td>
        </tr>
        <tr>
          <td colspan=2 style="text-align: center">
            <img style="border: 1px solid;background-color: aquamarine; width:60px; height:45px"/>
          </td>
        </tr>
      </table>
    </td>
  </tr>
  <tr>
    <td>
      <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>  
    <td>
      <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>
  </tr>
</table>
{% endcodeblock %}

效果如下：
![email_image_2](/images/email_image_2.png)

一个可行的解决方案是在所有要对齐的图片上都应用上相同的table span结构，试试下面的解决代码：
{% codeblock lang:html %}
<table>
  <tr>
    <td rowspan=2>
      <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>  
    <td></td>
    <td rowspan=2>
   	  <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align: center">
      <img style="border: 1px solid;background-color: aquamarine; width:60px; height:45px"/>
    </td>
  </tr>
  <tr>
   	<td rowspan=2>
   	  <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>
    <td></td>
    <td rowspan=2>
      <img style="border: 1px solid;background-color: aqua; width:200px; height:150px"/>
    </td>
  </tr>
</table>
{% endcodeblock %}

效果如下：
![email_image_3](/images/email_image_3.png)

相信你可以将这种模式推广应用于其他更加复杂的场景：）
