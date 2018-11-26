---
title: Bootstrap框架梳理
date: 2018-08-07 14:49:20
tags:
    - Bootstrap
category: html
---

Bootstrap简介:
> Bootstrap 是一个用于快速开发 Web 应用程序和网站的前端框架。Bootstrap 是基于 HTML、CSS、JAVASCRIPT
> 的。

优点:
移动设备优先 浏览器支持 易上手 响应式布局

<!-- more -->

 1. 基本的html结构
------------

 `<meta>`标签必须放在最前面
 META标签用来描述一个HTML网页文档的属性，例如作者、日期和时间、网页描述、关键词、页面刷新等
 
    ```
    <meta>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    ```
 基本模板
    ```
    <!DOCTYPE html>
    <html lang="zh-CN">
        <head>
            <meta charset="utf-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1">
            <title>bootstrap test</title>
            <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
        </head>
        <body>
            <h1>hello world</h1>


            <!-- 的所有 JavaScript 插件都依赖 jQuery,所以必须放在前边-->
            <script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
            <!-- 加载 Bootstrap的JavaScript 插件 -->
            <script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
        </body>
    </html>
    ```

2. 响应式布局
--------
布局容器
    .container 类用于固定宽度并支持响应式布局的容器
    .container-fluid 类用于 100% 宽度，占据全部视口（viewport）的容器
    container1170 左右padding15 实际显示1140
    row 左右margin-15 显示1170
预定义类:
    .row .col-xs  .text-left  .list-unstyled  .img-responsive .show .hidden .table-bordered 
组件:
    字体图标(css字体引入),

line-hight:1.5font-size
font-size:20px

3. 栅格
-----

https://v3.bootcss.com/css/#grid-example-basic
要点:
- column 12
- 嵌套列,被嵌套的行（row）所包含的列（column）的个数不能超过12
- 列排序-->上图下文字,右图左文字(小屏不变,大片偏移)

4. 媒体查询
-------


| 屏幕宽度 | container宽度 |
| :------: | :------: |
| col-xs <768 | auto | 
| col-sm 768-991 | 750px |
| col-md 992-1199 | 970px |
| col-lg >=1200 | 1170px |


5. 导航栏小屏 <760px
---------------

class id
data-target
data-toggle
细节


6. Carousel js轮播
----------------

https://v3.bootcss.com/javascript/#carousel