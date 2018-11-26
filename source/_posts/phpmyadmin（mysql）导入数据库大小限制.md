---
title: phpmyadmin（mysql）导入数据库文件大小限制
date: 2017-12-27 14:30
tags:
    - phpmyadmin
    - 数据库文件大小限制
category: php,mysql
---
默认情况下,phpMyAdmin导入MySQL数据库文件的大小为2M。
当我们需要上传的MySQL数据库文件大于2M时,系统就会提示错误(Warning: POST Content-Length of 50874526 bytes exceeds the limit of 8388608 bytes in Unknown on line 0)
解决方法如下:

<!-- more-->

- 找到php的配置文件php.ini
- 修改php.ini里面的上传文件的限制,数值根据你需要上传的数据库文件大小而定。
    - upload_max_filesize 上传文件大小
    - memory_limit 设置内存
    - post_max_size 提交数据的最大值
- 重启apache

<!-- more -->