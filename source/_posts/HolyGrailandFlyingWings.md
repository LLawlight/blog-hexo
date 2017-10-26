---
title: "圣杯布局与双飞翼布局的相同点与不同点"
date: 2016-03-14
categories: 
- 技术
- CSS
tags:
- 技术
- CSS
- 布局
---
  
三栏式布局是众多网页的常规布局之一，我们在采用三栏式布局时所提出的要求一般是左栏与右栏定宽，中间部分宽度自适应，中间部分作为内容主体，我们需要将其放在左栏与右栏之前来达到优先渲染的效果。      

2006年Matthew Levine在《A LIST APART》上发表了一篇名为[《In Search of the Holy Grail》](http://alistapart.com/article/holygrail#comments)的文章，提出了圣杯布局的思路。利用负外边距来实现我们需要的效果，充分体现了CSS的艺术与负外边距的强大。      

之后淘宝提出了为了实现同样的效果提出了双飞翼布局的思路，同样使用了负外边距来移动栏目从而达到目的。因此，圣杯布局和双飞翼布局有着相似的思路，但是在某些细节上也有不同点。

----------

# 相同点 #

首先，我们建立好左中右三栏，因为中间栏需要优先渲染，我们将其放在最前面。
```html
<div id="content">
  <div id="main">main</div>
  <div id="left">left</div>
  <div id="right">right</div>
</div>  
```
要实现左中右的水平排列，我们要将块状元素浮动。   
```css
#main, #left, #right {
  float: left;
}   
```
假设左栏定宽200px，右栏定宽120px。（为了看清楚效果，我们暂时为三栏设置一个背景色，同时设置不同的高度以便观察div之间是否重叠）
```css   
#main, #left, #right {
  float: left;
}
#left {
  background-color: red;
  height: 100px;
  width: 200px;
  margin-left: -100%;
}
#main {
  background-color: green;
  height: 150px;
  width: 100%;
}
#right {
  background-color: blue;
  height: 100px;
  width: 120px;
}
```
得到如下效果：   

![](https://ws1.sinaimg.cn/large/6ad0d67fgy1fkvfhajhiyj20i20cxwf8.jpg)   
   
接下来就要使用到强大的负外边距了，我们先处理左栏，因为中间栏的宽度为100%，所以左栏被挤到下面去了。现在left要到最左边的位置，所以我们在left的样式下输入：
```css	   
margin-left: -100%;
```

可以看到left已经顺利的被拉回来了。      

![](https://ws1.sinaimg.cn/large/6ad0d67fgy1fkvfhl8wjjj20i20a5js2.jpg)   
       
right同理，它靠在最右边。所以输入：
```css	   
margin-left：-120px；   
```
得到效果：      

![](https://ws1.sinaimg.cn/large/6ad0d67fgy1fkvfhwoa0zj20i2078q3k.jpg)

----------

**以上的步骤，圣杯布局和双飞翼布局是一模一样的。**

----------

# 不同点 #

----------
   
## 圣杯布局 ##
经过以上的设置，出现了一个问题，就是left、right都和main重叠了，我们需要把main缩回来，现在它的宽度充满了整个父元素content，并且随着父元素的变化而变化，我们对父元素进行设置。
```css
#content {
  padding: 0 120px 0 200px;
}
```
但是left和right都在content里，也受到影响被拉进来了。

![](https://ws1.sinaimg.cn/large/6ad0d67fgy1fkvfjchujuj20i20780td.jpg)

所以我们要分别让left往左移，right往右移到它们该在的位置上去，为了让它们移动，我们需要在left和right的样式下设置：
```css
position: relative;
```
并且对left设置：
```css	
left: -200px;
```
对right设置：
```css
right：-120px；
```
这样我们就能的到我们所想要的三栏式布局的效果了。


![](https://ws1.sinaimg.cn/large/6ad0d67fgy1fkvfjmbcoxj20i20783z7.jpg)

圣杯布局CSS代码：
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>三栏式布局</title>
    <style type="text/css">
      * {
        margin:0;
        padding:0;
      }
      #main, #left, #right {
        float: left;
      }
      #left {
        background-color: red;
        height: 100px;
        width: 200px;
        margin-left: -100%;
        position: relative;
        left: -200px;
      }
      #main {
        background-color: green;
        height: 150px;
        width: 100%;
      }
      #right {
        background-color: blue;
        height: 100px;
        width: 120px;
        margin-left: -120px;
        position: relative;
        right: -120px;
      }
      #content {
        padding: 0 120px 0 200px;
      }
    </style>
  </head>
  <body>
    <div id="content">
      <div id="main">main</div>
      <div id="left">left</div>
      <div id="right">right</div>
    </div>
  </body>
</html>
```
----------

## 双飞翼布局 ##

面临同样的问题，圣杯布局是对三栏的父元素的内边距进行了设置，而双飞翼布局则是从另一个角度入手。既然左右两边要被left和right独享，那main就不要这两边了，那就不会重叠了。      

我们创造一个新的div在main的里面，并且把内容都放在这个main的子元素里。      
```html
<div id="main"><div id="main_in">main</div></div>
```
并且给它设置左右的外边距，让出左右栏目的位置就可以实现我们需要的效果了。   
```css
#main_in {
  background-color: grey;
  height: 150px;
  margin-left: 200px;
  margin-right: 120px;
}
```
效果如下：   

![](https://ws1.sinaimg.cn/large/6ad0d67fgy1fkvfjwnmb7j20i207874x.jpg)   
   
（去掉main的背景色即可，效果图没有去掉是为了方便展示main_in的效果！）

双飞翼布局代码：
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>三栏式布局</title>
    <style type="text/css">
      * {
        margin:0;
        padding:0;
      }
      #main, #left, #right {
        float: left;
      }
      #left {
        background-color: red;
        height: 100px;
        width: 200px;
        margin-left: -100%;
      }
      #main {
        background-color: green;
        height: 150px;
        width: 100%;
      }
      #right {
        background-color: blue;
        height: 100px;
        width: 120px;
        margin-left: -120px;
      }
      #main_in {
        background-color: grey;
        height: 150px;
        margin-left: 200px;
        margin-right: 120px;
      }
    </style>
  </head>
  <body>
    <div id="content">
      <div id="main"><div id="main_in">main</div></div>
      <div id="left">left</div>
      <div id="right">right</div>
    </div>
  </body>
</html>
```
----------
效果在线预览：[http://zhaoyuxiang.cn/task1-3/](http://zhaoyuxiang.cn/task1-3/)