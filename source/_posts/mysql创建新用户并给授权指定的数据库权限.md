---
title: mysql创建新用户并给授权指定的数据库权限
date: 2017-11-19 11:25:42
tags:
    - mysql
    - mysql授权
category: php,mysql
---

<!-- more-->

### 一、要求:
`
创建新用户为:new_user 密码设置为:newpassword
`
### 二: 步骤:
1. 终端登录数据库
`
mysql -uroot -p
`

2. 创建用户:
` 
CREATE USER '新用户用户名'@'%' IDENTIFIED BY '密码'; 
` 
例如:
` 
CREATE USER 'new_user'@'%' IDENTIFIED BY 'newpassword'; 
` 

- '%' -- 所有情况都能访问

- 'localhost' -- 本机才能访问

- '111.222.33.44' -- 指定 ip 才能访问


3. 创建数据库:
`
create database new_lmlq;
`
4. 为新用户授权
`
grant 权限 on 想授权的数据库.* to '新用户'@'%';
`
`
grant all privileges on 想授权的数据库.* to '新用户'@'%';
`
`
grant all privileges on new_lmlq.* to 'new_lmlq'@'%';
`

*all 可以替换为 select,delete,update,create,drop*

5. 刷新系统权限表
`
flush privileges;
`
6. 导入SQL文件
`
source sql.sql
`