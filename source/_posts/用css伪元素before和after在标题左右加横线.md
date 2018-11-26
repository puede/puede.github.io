---
title: 用css伪元素before和after在标题左右加横线
date: 2018-07-19 09:52:29
tags:
    - before,after
    - css伪元素
category: css

---
:before 和:after是css的两个伪元素用于向选定的元素前后插入内容。

效果如下:

![效果图][1]


  [1]: https://raw.githubusercontent.com/puede/font/master/title-1.JPG

  <!-- more -->
  
  html:

    <h1 class="common-title">标题文字</h1>

  sass: 

    .common-title
        color: #0da056
        font-family: yrzt
        font-size: 30px
        width: 280px //给标题一个固定宽度,
        margin: 0 auto // 居中
        &:after,&:before
            content: ""
            border-top: 1px solid #0da056
            position: absolute
        &:after
            width: 116px // 标题两侧线的宽度
            margin-top: 16px
            margin-left: 15px // 
        &:before
            width: 116px
            margin-top: 16px
            margin-left: -132px
            //margin-left是横线距标题的距离,这样写的好处是自适应响应式布局,不在因为屏幕的不同来回改变间距
            