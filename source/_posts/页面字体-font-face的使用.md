---
title: 页面字体@font-face的使用
date: 2018-07-16 09:38:55
tags:
    - font-face
    - font-family
category: css

---
字体使用是网页设计中不可或缺的一部分。经常地，我们希望在网页中使用某一特定字体，但是该字体并非主流操作系统的内置字体，这样用户在浏览页面的时候就有可能看不到真实的设计。美工设计师最常用的办法是把想要的文字做成图片，但这样做有几个明显的缺陷：

 1. 不能大范围地使用该字体
 2. 图片内容相对使用文字不易修改
 3. 不利于网站搜索引擎优化


 <!-- more-->

下面要讲的是如何只通过CSS的@font-face属性来实现在网页中嵌入任意字体。
首先获取要使用字体的3种文件格式，确保在主流浏览器中都能正常显示该字体。

1. TTF或OTF，适用于Firefox 3.5、Safari、Opera。
2. EOT，适用于InternetExplorer 4.0+。
3. SVG，适用于Chrome、iPhone

@font-face规则在CSS3规范中属于字体模块，用较为专业的话来讲，@font-face能够加载服务器端的字体文件，让客户端浏览器显示客户端没有安装的字体。如果没有@font-face规则，浏览器只能够在客户端系统中寻找指定字体，这就给网页设计带来了很多限制，妨碍了设计师的创意设计，也就无法展现丰富多彩的字体艺术。
下面要解决的是如何获取某种字体的这3种格式文件。一般地，我们在手头上（或在设计资源站点已经找到）有该字体的某种格式文件，最常见的是TTF文件，我们需要通过对这种文件格式进行转换来得到其余两种文件格式。字体文件格式的转换可以通过网站FontsQuirrel或onlinefontconverter提供的在线字体转换服务实现。这里推荐第一个站点，它允许我们选择需要的字符生成字体文件（在服务的最后一个选项），从而大大缩减了字体文件的大小，使得本方案更具实用性。字体声明如下。
 

        <!-- 引入字体文件ttf/eot -->
        @font-face{
           font-family: myFont;
           /*eot格式兼容IE*/
           src:url(fonts/zdy.eot);
           /*ttf格式兼容非IE*/
           src:url(fonts/zdy.ttf);
        }
        h1{
           font-family: myFont;
        }

举例:
1. 下载字体https://github.com/puede/font/blob/master/zcool.ttf
2. 引入字体文件并使用

<!--  -->
    @font-face{
        font-family: 'yrzt';
        src: url('/fonts/zcool.ttf');
    }
    h1{
        font-family: 'yrzt';
    }
页面效果: 
![字体图片][1]


  [1]: https://raw.githubusercontent.com/puede/font/master/zcool.jpg

  参考:
  https://mp.weixin.qq.com/s/U3IzYblkfP2_qxg0nT_1Vw
