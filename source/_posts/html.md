---
title: html
date: 2024-08-11 00:41:54
tags:
  - Html
---
# 盒子模型
![images](../images/html/Standard-box-model.png)
![images](../images/html/Weird-box-model.webp)


# 元素的offsetHeight、scrollHeight、clientHeight属性

offsetHeight: 包含元素的边框、内边距、内容和元素的水平滚动条（如果存在且渲染的话）
![images](../images/html/dimensions-offset.png)



offsetTop: 仅仅是元素边框之间的距离，当前元素相对于最近的定位父元素或者最近的 `table`, `td`, `th`, `body` 元素
![images](../images/html/offsetTop.webp)





scrollHeight: 只读属性 包括由于溢出导致的视图中不可见内容 即包括元素的内边距、内容高度
![images](../images/html/scrollheight.png)



scrollTop: 可以获取或设置元素内容从其顶部边缘滚动的像素数
![images](../images/html/scrollTop.webp)





clientHeight: 通过 CSS `height` + CSS `padding` - 水平滚动条高度（如果存在）来计算
![images](../images/html/dimensions-client.png)



clientTop: 获取元素顶部边框的宽度（以像素表示）