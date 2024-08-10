---
title: css学习记录
tags:
  - Css
---



# [flex布局](https://www.w3school.com.cn/css/css3_flexbox.asp)

- `flex-direction` 定义容器要在哪个方向上堆叠 flex 项目
- `flex-wrap`  规定是否应该对 flex 项目换行
- `flex-flow `   用于同时设置 flex-direction 和 flex-wrap 属性的简写属性
- `flex` 用于同时设置`flex-grow`(默认0) `flex-shrink` (默认1) `flex-basis` (默认auto)
- `justify-content`   用于对齐 flex 项目 （一行 左右方向操作）
- `align-items ` 用于垂直对齐 flex 项目  (不换行的时候 上下方向操作)
- `align-content ` 用于对齐弹性线 （换行得时候 上下方向操作）

```
flex-flow: flex-direction flex-wrap
flex: flex-grow(有剩余宽度的情况 0) flex-shrink(超出规定宽度的情况 1) flex-basis(基本的宽度 auto flex-basis比width具有更高的优先级)
```

**align-self**：auto | flex-start | flex-end | center | baseline | stretch

![image-20240323194835754](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240323194835754.png)

# li元素

list-style: <' [list-style-type](https://james-curtis.github.io/css-handbook/properties/list/list-style-type.htm) '> || <' [list-style-position](https://james-curtis.github.io/css-handbook/properties/list/list-style-position.htm) '> || <' [list-style-image](https://james-curtis.github.io/css-handbook/properties/list/list-style-image.htm) '>

disc outside none



# z-index

定位元素。即定义了[position](https://eric-gitta-moore.github.io/css-handbook/properties/positioning/position.htm)为`非static`的元素



# transform 

##### transform: translate(-50%);是参考哪个元素的50%  参考自身的宽高

`transform: translate(-50%,-50%);` 表示将元素沿着其自身的宽度和高度的一半进行平移。

需要注意的是，这种技术通常与绝对定位（absolute positioning）或固定定位（fixed positioning）一起使用，以便相对于其父元素进行居中定位。

```css
.big{
    width: 300px;
    height: 300px;
    background-color: red;
    position: relative;
}

.small{
    width: 100px;
    height: 100px;
    background-color: aqua;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```



[`transform`](https://www.w3school.com.cn/css/css3_2dtransforms.asp) 变换  translate左右移动

`transition` 过渡   元素慢慢变化

```css
transition-property: width;
transition-duration: 2s;
transition-timing-function: linear;
transition-delay: 1s;
```



`animation` 动画   none 0 ease 0 1 normal

```css
animation: name duration timing-function(动画的速度) delay iteration-count direction;
 
@keyframes 定义关键帧

name: 动画名
duration: 持续时间
iteration-count：设置动画应运行多少次  infinite 无限次
direction：反向或交替运行动画  normal - 动画正常播放（向前）。默认值
                            reverse - 动画以反方向播放（向后）
                            alternate - 动画先向前播放，然后向后
                            alternate-reverse - 动画先向后播放，然后向前
timing-function(动画的速度)
                            ease - 指定从慢速开始，然后加快，然后缓慢结束的动画（默认）
                            linear - 规定从开始到结束的速度相同的动画
                            ease-in - 规定慢速开始的动画
                            ease-out - 规定慢速结束的动画
                            ease-in-out - 指定开始和结束较慢的动画
                            cubic-bezier(n,n,n,n) - 运行您在三次贝塞尔函数中定义自己的值
```





# grid布局

[链接](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

### justify 横向

### align 纵向





# background





# [伪类&伪元素](https://www.w3school.com.cn/css/css_pseudo_elements.asp)

`:link`  未浏览

`:visited`  已浏览

`:hover`  鼠标放上去

`:active` 点击得时候

: 伪类

:: 伪元素



# nth-child&nth-of-type()

:nth-child(n) :nth-last-child(n)(倒着数) :first-child :last-child  表示在每一组有父子结构的都会生效

:first-of-type :last-of-type :nth-of-type(n) :nth-last-of-type(n)  匹配父元素的倒数第n个子元素

[`first-child`与`first-of-type`的区别](https://www.cnblogs.com/2050/p/3569509.html) 

**:first-child** 匹配的是某父元素的第一个子元素，可以说是结构上的第一个子元素。

**:first-of-type** 匹配的是某父元素下相同类型子元素中的第一个，比如 p:first-of-type，就是指所有类型为p的子元素中的第一个。这里不再限制是第一个子元素了，只要是该类型元素的第一个就行了。

同样类型的选择器 :last-child 和 :last-of-type、:nth-child(n) 和 :nth-of-type(n) 也可以这样去理解。



# 属性选择器

[链接](https://www.w3school.com.cn/css/css_attribute_selectors.asp)

- [attribute|="value"] 选择器用于选取指定属性以指定值开头的元素

- [attribute^="value"] 选择器用于选取指定属性以指定值开头的元素 值不必是完整单词
- [attribute$="value"] 选择器用于选取指定属性以指定值结尾的元素 值不必是完整单词
- [attribute*="value"] 选择器选取属性值包含指定词的元素 值不必是完整单词





# position

[链接](https://www.w3school.com.cn/css/css_positioning.asp)

- static                                              top、bottom、left 和 right不起作用

- relative                                          相对于其正常位置进行定位

- fixed                                               固定某个元素不动   top、bottom、left 和 right进行调整位置在哪固定

- absolute                                        相对于最近的定位祖先元素（除了static都算定位元素）进行定位（而不是相对于视口定位，如          fixed）如果绝对定位的元素没有祖先，它将使用文档主体（body），并随页面滚动一起移动

- sticky                                              起先它会被相对定位，直到在视口中遇到给定的偏移位置为止 - 然后将其“粘贴”在适当的位置（比如  

  ​                                                        position:fixed）

absolute和fixed会脱离文档流





# margin塌陷

<https://juejin.cn/post/6976272394247897101>

父子嵌套的元素垂直方向的`margin`取最大值

解决margin塌陷方法

![image-20240105224715948](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240105224715948.png)

margin合并

两个上下相邻元素的外边距取最大值

# rem和em区别

rem与em都是相对单位，我们使用它们的目的就是为了适应各种不同的移动端和pc端的屏幕。 **rem是根据html根节点来计算的，而em是根据父级元素的字体计算的**。 简单概括就是: rem相对于根元素,   em相对于父元素字体



# 预处理器&后处理器

less sass cssnext



autoprefixer



# box-shadow

box-shadow: h-shadow v-shadow blur spread color inset;

### 属性值

| 值         | 描述                                     | 测试                                                         |
| :--------- | :--------------------------------------- | :----------------------------------------------------------- |
| *h-shadow* | 必需。水平阴影的位置。允许负值。         | [测试](https://www.w3school.com.cn/tiy/c.asp?f=css_box-shadow) |
| *v-shadow* | 必需。垂直阴影的位置。允许负值。         | [测试](https://www.w3school.com.cn/tiy/c.asp?f=css_box-shadow) |
| *blur*     | 可选。模糊距离。                         | [测试](https://www.w3school.com.cn/tiy/c.asp?f=css_box-shadow&p=3) |
| *spread*   | 可选。阴影的尺寸。                       | [测试](https://www.w3school.com.cn/tiy/c.asp?f=css_box-shadow&p=7) |
| *color*    | 可选。阴影的颜色。请参阅 CSS 颜色值。    | [测试](https://www.w3school.com.cn/tiy/c.asp?f=css_box-shadow&p=10) |
| inset      | 可选。将外部阴影 (outset) 改为内部阴影。 | [测试](https://www.w3school.com.cn/tiy/c.asp?f=css_box-shadow&p=15) |

| 默认值： | none |
| -------- | ---- |





# grid

- [grid-template-rows](https://www.w3school.com.cn/cssref/pr_grid-template-rows.asp)
- [grid-template-columns](https://www.w3school.com.cn/cssref/pr_grid-template-columns.asp)
- [grid-template-areas](https://www.w3school.com.cn/cssref/pr_grid-template-areas.asp) 要设置grid-area属性
- [grid-auto-rows](https://www.w3school.com.cn/cssref/pr_grid-auto-rows.asp)  grid-template-rows 属性覆盖此属性
- [grid-auto-columns](https://www.w3school.com.cn/cssref/pr_grid-auto-columns.asp)  grid-template-columns 属性会覆盖此属性
- [grid-auto-flow](https://www.w3school.com.cn/cssref/pr_grid-auto-flow.asp)  两个属性 row column



grid-area: 语法是 grid-row-start / grid-column-start / grid-row-end / grid-column-end
默认值 auto / auto / auto / auto	

grid: none|grid-template-rows / grid-template-columns|grid-template-areas|grid-template-rows / [grid-auto-flow] grid-auto-columns|[grid-auto-flow] grid-auto-rows / grid-template-columns|initial|inherit;

## 属性值

| 值                                                          | 描述                                                         |
| :---------------------------------------------------------- | :----------------------------------------------------------- |
| none                                                        | 默认值。不定义行或列的尺寸。                                 |
| *grid-template-rows* / *grid-template-columns*              | 规定列和行的尺寸。                                           |
| *grid-template-areas*                                       | 规定使用命名项目的网格布局。                                 |
| *grid-template-rows* / *grid-auto-columns*                  | 规定行的尺寸（高度），以及列的自动尺寸。                     |
| *grid-auto-rows* / *grid-template-columns*                  | 规定行的自动尺寸，并设置 grid-template-columns 属性。        |
| *grid-template-rows* / *grid-auto-flow* *grid-auto-columns* | 规定行的尺寸（高度），以及如何放置自动就位的项目，和列的自动尺寸。 |
| *grid-auto-flow* *grid-auto-rows* / *grid-template-columns* | 规定如何放置自动就位的项目，和行的自动尺寸，以及设置 grid-template-columns 属性。 |
| initial                                                     | 将此属性设置为其默认值。参阅 [initial](https://www.w3school.com.cn/cssref/css_initial.asp)。 |
| inherit                                                     | 从其父元素继承此属性。参阅 [inherit](https://www.w3school.com.cn/cssref/css_inherit.asp)。 |

## 技术细节

| 默认值： | none none none auto auto row |
| -------- | ---------------------------- |





# 实现图片文字居中

vertical-align



# 如何实现盒子内容水平居中

- 行内元素或者内联元素 使用父元素的 `text-align` 属性来实现文本水平居中

  ```css
  .container {
    text-align: center;
  }
  ```

- 块级元素 使用`margin: auto;` 来使元素水平居中

  ```css
  .container {
    margin-left: auto;
    margin-right: auto;
  }
  
  .container {
    margin: auto;
  }
  ```

- 使用 flex 布局

  ```css
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  ```

- 使用 grid 布局

  ```css
  .container {
    display: grid;
    place-items: center;
  }
  ```

- 使用transform

  ```css
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  ```

  

# 如何实现两栏分开布局

#### 1. 流式布局（使用 `float`）：

```css
.container {
  width: 100%;
}

.left-column {
  width: 70%;
  float: left;
}

.right-column {
  width: 30%;
  float: left;
}
```

#### 2. Flex 布局：

```css
.container {
  display: flex;
}

.left-column {
  flex: 70%;
}

.right-column {
  flex: 30%;
}
```

#### 3. Grid 布局：

```css
.container {
  display: grid;
  grid-template-columns: 70% 30%;
}
```

# 如何实现三栏布局

### 1. 流式布局（使用 `float`）：

```
.container {
  width: 100%;
}

.left-column {
  width: 30%;
  float: left;
}

.center-column {
  width: 40%;
  float: left;
}

.right-column {
  width: 30%;
  float: left;
}
```

### 2. Flex 布局：

```
.container {
  display: flex;
}

.left-column {
  flex: 30%;
}

.center-column {
  flex: 40%;
}

.right-column {
  flex: 30%;
}
```

### 3. Grid 布局：

```
.container {
  display: grid;
  grid-template-columns: 30% 40% 30%;
}
```

### 4. 圣杯布局（使用 `flex` 和负边距）：

```
.container {
  display: flex;
}

.left-column,
.right-column,
.center-column {
  flex: 1;
}

.center-column {
  order: 2;
}

.left-column {
  order: 1;
  margin-right: -100%;
}

.right-column {
  order: 3;
  margin-left: -100%;
}
```

### 5. 双飞翼布局（使用 `float` 和 `margin`）：

```
.container {
  width: 100%;
}

.column {
  float: left;
  width: 100%;
}

.left-column {
  width: 30%;
  margin-left: -100%;
}

.center-column {
  width: 40%;
}

.right-column {
  width: 30%;
  margin-left: -30%;
}
```



# 块级格式化上下文

BFC 主要用于处理布局和浮动等问题，它规定了块级盒子如何堆叠、定位和清除浮动



创建 BFC 的方式有多种，其中一些包括：

- **根元素：** HTML 文档的根元素（html）就是一个 BFC。
- **浮动元素：** 元素的 float 属性不是 none。
- **绝对定位元素：** 元素的 position 属性为 absolute 或 fixed。
- **display 属性值不为 block、inline-block、flex、inline-flex 之一的元素：** 例如，table 元素会生成一个 BFC。
- **overflow 属性不为 visible 的元素：** 设置了 overflow 属性值为 auto、scroll、hidden 的元素会生成 BFC。



# 尺寸的百分比

普通元素相对于父元素的内容区域  （没有定位的元素称为普通元素）

绝对 (固定) 定位元素的参考系为父元素中 第一个定位元素的padding区域



# 隐藏元素的三种方式

如果需要彻底隐藏元素且不占据页面空间，可以使用 `display: none;`；

如果需要隐藏元素但仍然占据页面空间，可以使用 `visibility: hidden;`；

如果需要隐藏元素但仍然保留占据的空间且可能需要进行过渡效果，可以使用 `opacity: 0;`



# css-loader&style-loader&less-loader&sass-loader

由于css-loader仅提供了将css转换为字符串导出的能力

style-loader可以将css-loader转换后的代码进一步处理，将css-loader导出的字符串加入到页面的style元素中



BEM  &  CSS in js  & CSS module

BEM全称是：**B**lock **E**lement **M**odifier

BEM是一套针对css类样式的命名方法

三个部分的具体含义为：

- **Block**：页面中的大区域，表示最顶级的划分，例如：轮播图(```banner```)、布局(```layout```)、文章(```article```)等等
- **element**：区域中的组成部分，例如：轮播图中的横幅图片(```banner__img```)、轮播图中的容器（```banner__container```）、布局中的头部(```layout__header```)、文章中的标题(```article_title```)
- **modifier**：可选。通常表示状态，例如：处于展开状态的布局左边栏（```layout__left_expand```）、处于选中状态的轮播图小圆点(```banner__dot_selected```)



css in js 的核心思想是：用一个JS对象来描述样式，而不是css样式表

```js
const styles = {
    backgroundColor: "#f40",
    color: "#fff",
    width: "400px",
    height: "500px",
    margin: "0 auto"
}
```



# postcss

真正起作用的是里面的插件```postcss-preset-env```

```js
module.exports = {
    plugins: {
        "postcss-preset-env": {} // {} 中可以填写插件的配置
    }
}
```





# border

https://www.w3school.com.cn/css/css_border_shorthand.asp

border:  三个属性简写

- `border-width`
- `border-style`（必需）
- `border-color`



以切角的形式相连  比如做个三角形