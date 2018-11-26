---
title: 利用php正则表达式抓取页面内容存入数据库
date: 2018-07-27 10:43:47
tags:
    - 正则表达式
    - 数据库
    - php
category: php

---


第一步:获取整个页面内容
------------




我们知道页面抓取最常用的两种方式file_get_contents()函数和curl抓取

<!-- more -->

 1. file_get_contents()
 2. curl抓取, curl是一个非常强大的开源库,支持很多协议,包括HTTP FTP TELNET等,我们使用它来发送HTTP请求.它给我们带来的好处是可以通过灵活的选项设置不同的HTTP协议参数,并且支持HTTPS,curl可以根据URL前缀是HTTP还是HTTPS自动选择是否加密发送内容 ,curl尤其是在api接口调用过程中用处广泛
我用的是file_get_contents(),这里我们先说file_get_contents(),以后有空再说curl

废话不多说,看代码:
方法一:

    $news=file_get_contents("http://news.baidu.com/");

方法二:

    $url="http://news.baidu.com/";
    $news=file_get_contents($url);

就是这么简单.这样就把整个页面抓取下来了

我们也可以把它存入另一个文件.不过在这没什么用

    $file=fopen("./fwrite.php","w");
    fwrite($file,$youku);
    fclose($file);




第二步:利用正则匹配想要得到的内容
-----------------



这里我们以抓取百度新闻为例,那就要抓取代码<a>标签里面的内容
preg_match()和preg_match_all()的区别:
前者是在搜索到第一个匹配内容之后就停止搜索,而后者在搜索到第一个匹配内容之后还会一直搜索直到搜索到所有的匹配结果,所以我们这里用preg_match_all()

    preg_match_all("/<a.*?href.*?=.*?[\"].*?[\"].*>.*<\/a>/",$news,$matches);// $matches是匹配到的结果

这里是匹配到的部分结果

    [9] => <a href="/">首页</a>
    [10] => <a href="/guonei">国内</a>
    [11] => <a href="/guoji">国际</a>
    [12] => <a href="/mil">军事</a>
    [13] => <a href="/finance">财经</a>
    [14] => <a href="/ent">娱乐</a>

 我这里得到的$matches是一个二维数组,而且还带有`<a href=""></a>`这样的标签,我们先把它转化为字符串,然后去除多余的标签,再转为数组

    $str=implode(",",$matches1[0]);
    $striptag=strip_tags($str);
    $arr=explode(",",$stiptag);
    
如下:

    [0] => 网页
    [1] => 贴吧
    [2] => 知道
    [3] => 音乐
    [4] => 图片
    [5] => 视频
    [6] => 地图




第三步:存入数据库
---------



首先连接数据库,如果连接成功,就把数据存入数据表table_news,table_news只有两个字段:自增id和content
mysqli_connect()参数说明:
mysqli_connect('连接地址','用户名','密码','数据库','端口');

    $connect= mysqli_connect('localhost','root','123456','regex','3306');
    if ($connect){
        foreach ($arr as $key => $value) {
            $sql ="insert into table_news values ('','$value')";
            $res=$connect -> query($sql);
            if ($res){
                echo $value.'插入数据表成功<br>';
            }
        }
    }
    
ok了
    
    
    
    
    
    
    
    
    
    