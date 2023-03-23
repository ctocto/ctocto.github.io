---
title: 改变 Chrome autocomplete CSS style, -webkit-autofill
date: 2023-03-23 20:30:00
tags: [CSS,autocomplete,autofill]
categories: 饭碗(技术)
---

```css
input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus,
input:-webkit-autofill:active {
  /* 1 + 2 较完善一些 */
  /* 1、会有闪烁 */
  -webkit-background-clip: text;
  -webkit-text-fill-color: #fff !important;
  /* 2、固定时间的动画，时间到了会显露出来 */
  transition: background-color 600000s 0s, color 600000s 0s; 
}
```
