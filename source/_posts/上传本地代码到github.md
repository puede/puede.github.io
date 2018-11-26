---
title: 上传本地代码到github
date: 2017-11-16 15:48:02
tags:
正文: 

---
<!-- 上传本地代码到github -->

<!-- perla -->
<!-- more-->
 第一步. 建立git仓库--在本地项目根目录下执行git命令

> git init

 第二步. 将项目的所有文件添加到git仓库中
 
> git add .

 第三步. 将add的文件commit到仓库
 
> git commit -m"注释"

 第四步. 在github上创建自己的Repository，创建页面如下图所示：

![create-repository](https://raw.githubusercontent.com/puede/tupian/master/create-pro.png)
 
 
第五步. 点击create-repository 

![此处输入图片的描述](https://raw.githubusercontent.com/puede/tupian/master/created.PNG)

第六步. 将本地的仓库关联到github上

> git remote add origin git@github.com:puede/puede.git

![此处输入图片的描述](https://raw.githubusercontent.com/puede/tupian/master/1.png)

第七步. 上传github之前,先pull一下

> git pull origin master

第八步. 上传代码到github

> git push -u origin master

---
