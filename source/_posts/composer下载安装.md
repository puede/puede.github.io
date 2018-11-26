---
title: Composer下载安装
date: 2017-11-16 17:24:15
tags:   
    - composer
    - composer安装
category: php
---
Composer是什么?

简单来说，Composer 是一个新的安装包管理工具，服务于 PHP 生态系统。它实际上包含了两个部分：Composer 和 Packagist。

<!-- more-->

### 1.Composer绿色安装

下载 composer.phar  
```
  https://getcomposer.org/download/   
  ```
  ![composer.phar](https://raw.githubusercontent.com/puede/tupian/master/composer-1.PNG)

下载完成后将composer.phar 拷贝到  wamp/php 目录下,
然后目录下(wamp/php )新建 composer.bat 文件,编辑并写入以下内容
```
@php %~dp0composer.phar %*
```
保存并右击以管理员身份运行,如果运行出错,打开php.ini 找到无关的  .dll文件 注释 如php_pdo_firebird.dll。
把你的php路径加入到环境变量, 在新的终端中输入 composer 出现以下结果,则安装成功。

![composer安装](https://raw.githubusercontent.com/puede/tupian/master/composer-2.PNG)

### 2. 使用

可以使用以下命令下载thinkphp5

```
composer create-project topthink/think tp5 --prefer-dist
```
安装成功,
![composer安装tp5成功](https://raw.githubusercontent.com/puede/tupian/master/%E4%B8%8B%E8%BD%BDtp5.jpg)

如果出现以下错误

![composer安装tp5](https://raw.githubusercontent.com/puede/tupian/master/composer-3.png)

解决方法:

php.ini中  extension=php_openssl.dll  解除注释  (记得重启apache)
或者终端中 输入composer config -g -- disable-tls true 

### 3.composer声明依赖关系
在项目根目录下(例如:D:\wamp\apache2\htdocs\git)新建文件夹phpexcel,里面建一个composer.json 写入
```
{
    "require": {
        "monolog/monolog": "1.2.*",

        "phpoffice/phpexcel": "1.8.*"
    }
}
```
![依赖关系](https://raw.githubusercontent.com/puede/tupian/master/composer%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.jpg)

然后composer.install 在文件夹下就会看到新增的文件

然后在D:\wamp\apache2\htdocs\git\phpexcel文件夹下新建index.php写入
```
<?php
   
    require 'vendor/autoload.php';
    $objPHPExcel=new PHPExcel();
    var_dump($objPHPExcel);
?>
```
这段代码的作用是引入自动加载文件,
然后在浏览器访问你的phpexcel的地址,就会出现自动加载的内容,测试成功。