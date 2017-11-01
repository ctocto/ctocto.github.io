---
title: position:sticky 的使用 
date: 2017-04-01 10:28:09
tags: [css,移动端,pc,position]
categories: 饭碗(技术)
---

![效果](http://image.tf56.com/dfs/group1/M00/3B/C1/CiFBCVn5MtiAXEZ9AANVkO5kPvw994.gif)


如果你想想做这样一个滚动跟随。那你肯定第一时间想到的一定是 **position:fixed;** 这个css 属性，然后加上一段js 的代码。

这个方式是可以的对于pc 浏览器和Android 浏览。但是对于ios 设备上的浏览器来说，它是不好的，它的反应会很慢，当你的滚动已经达到了你想要它浮起来的时候，它却没有，超出好多才浮到顶部。当然这并非是你因你的js  代码写的不够好。

此时，你就应该用 **position:sticky** 这个样式了。

你只需要这样做：

判断浏览器是否支持sticky  这个属性，如果支持，给想要浮动跟随的 DOM 加上 
```
.sticky{
    position: -webkit-sticky;
    position: sticky;
    top: 0;
    z-index: 20;
}
```
这样一个样式就好了，不需要js 代码。

不支持的设备你就需要用  position:fixed; 的了，然后加上js 的判断代码，这里就不说js 代码了。

一个判断是否支持sticky 的方式：
```
var sticky = (function () {
                 var vendorList = ['', '-webkit-', '-ms-', '-moz-', '-o-'],
                      vendorListLength = vendorList.length,
                      stickyElement = document.createElement('div');

                  for (var i = 0; i < vendorListLength; i++) {
                      stickyElement.style.position = vendorList[i] + 'sticky';
                      if (stickyElement.style.position !== '') {
                          return true;
                      }
                  }
                  return false;
              })();
```

##### Caniuse 兼容图
![caniuse](http://image.tf56.com/dfs/group1/M00/3B/C3/CiFBCln5MsaAf1HRAAEnHj11z2w245.png)

由此图可以看出也并不是所有的 **Android  设备**  不支持  ， **ios 设备**  也并非全部支持。所以根据设备来判断，可能有误差。可以说高级浏览器是支持。

####备注

1、 使用sticky 样式的 元素  的  父级 及 其 祖先元素 的 overflow  得是  默认的 visible；
2、 父级 及 其 祖先元素 高度 与其 一样高，是没有效果的。