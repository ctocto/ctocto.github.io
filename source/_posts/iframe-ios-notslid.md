---
title: iframe 在ios上无法滚动问题
date: 2017-03-05 15:29:46
tags: [iframe,ios,移动端]
categories: 饭碗(技术)
---

### iframe 在ios上无法滚动问题
#### 场景：
A页面中用iframe 来嵌套B页面，而B页面的内容很多（超出一屏），想要的展示效果是看不出嵌套，正常滑动查看全部页面，所以给iframe设置高度与屏幕一样高。那么问题来：
安卓手机上完美无缺，而ios上不能滑动，就像是给一个div 设置了overflow:hidden;一样，超出的内容不能展示了。
那么我们去掉高度，发现ios上自动撑开了，跟B页面一样高。而安卓上发现iframe 高度是一个默认高度。
这样一来，我们需要判断设备来坐兼容了吗？显然在是不明智。
我们只需要这样：

```
<div class="page_box">
    <iframe id="iframe1" src="">  
    </iframe> 
</div>

```
```
.page_box{
     -webkit-overflow-scrolling: touch;  
     overflow-y: scroll;  
 }

```
给iframe 外边套一层，给这个容器设置这样的样式，给iframe 设置跟屏幕一样高度就OK了。