---
title: new Date() 时间的坑
date: 2017-03-20 11:20:16
tags: [js,移动端,时间]
categories: 饭碗(技术)
---
### new Date() 时间的坑
``字符串转换时间`` 

后端返回字段  createdate: "2016-08-19 11:28:39"
而我只需要月和日，怎么办？
说说我一开始是怎么实现的：

```
var createDate = new Date("2016-08-19 11:28:39");
```

![Alt text](http://image.tf56.com/dfs/group1/M00/3B/C1/CiFBCVn5MiGAe-T5AAAWDeqD6bE637.png)

哈哈，玩的好愉快，chrome 没问题，android（4.4.4） 没问题，掉咋天。iphone（6，8.4.1） 返回  **Invalid Date **
查看相关文档，发现js 根本不知道这么做，可能某些浏览器 发展的比较牛逼，功能强大，就上天了。iphone 是用不了了。
解决方案：

```
var createDate = "2016-08-19 11:28:39".replace(/-/g,"/");
createDate = new Date(createDate);
```

却别就是  【 - 】  这个符号他是不支持的，  【/】 这个他是可以得。
然后进行   getDate() , getMonth()   等操作，就会飞了……