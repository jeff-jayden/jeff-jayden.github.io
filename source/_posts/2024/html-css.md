---
title: html_css
comments: true
date: 2024-12-06 20:33:08
tags:
---


如果父元素是body，且没有给body和html写特殊样式。width的100%就相当于100vw了。因为html和body默认的width是auto（且是block，所以相当于width是fill），body默认会有宽度。所以100%就相当于100vw。
但是如果父元素是body，且没有给body和html写特殊样式。设置height 100%就不行。因为html和body的height默认值都是auto，具体高度是由子元素决定的。所以如果你需要设置高度，建议用100vh。

https://juejin.cn/post/7113190608071557157


body,
html,
#app {
  height: 100%;
  width: 100%;
}