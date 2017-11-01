---
title: pc text-overflow:ellipsis; IE10 以下不生效
date: 2017-03-05 15:42:46
tags: [pc,IE,css3]
categories: 饭碗(技术)
---
### pc text-overflow:ellipsis; IE10 以下不生效

![Alt text](http://image.tf56.com/dfs/group1/M00/3B/C3/CiFBCln5Mx6AZ_8XAAAeGp38dNc801.png)

```
.txt-hide{
    width:100%;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}

```
实现这样一个效果，发现iE10 以下包含10 不生效。
查了一下兼容性，到ie8 都是没有问题的。

排查发现 是因为继承了 word-wrap: break-word; 样式所导致的。
修改之后为：

```
.txt-hide{
    width:100%;
    white-space: nowrap;
    word-wrap: normal!important;
    text-overflow: ellipsis;
    overflow: hidden;
}

```