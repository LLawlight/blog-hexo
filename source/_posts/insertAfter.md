---
title: "原生JS之insertAfter：在现有元素后插入一个新元素"
date: 2016-03-10
categories: 
- 技术
- JavaScript
tags:
- 技术
- JavaScript
---

在学习DOM时我们知道有一个方法能将一个新元素插入到一个现有元素的前面，那就是insertBefore()，它的调用语法为：   
```js   
parentElement.insertBefore(newElement, targetElement);
```
<!-- more -->

可以看出，在使用此方法时，我们必须提供三个元素给它：   
1. 你想插入的新元素newElement。   
2. 你想把这个新元素插入到哪个目标元素（targetElement）之前。   
3. 目标元素的父元素parentElement。

----------
既然有insertBefore的方法，那也应该需要有个insertAfter，DOM并没有提供这种方法，我们可以自己动手写一个，将这个函数方法放入自己的javascript库里，以备不时之需。   
参照insertBefore，我们需要提供三个元素给insertAfter()，其中目标元素的父元素可以用targetElement.parentNode来代替。那么insertAfter()有两个参数，具体代码如下：   
```js
function insertAfter(newElement, targetElement) {
  var parent = targetElement.parentNode;
  if(parent.lastChild == targetElement) {
    //如果目标元素是其父元素的最后一个元素，则把新元素变成父元素的最后一个元素
    parent.appendChild(newElement);
  } else {
    //如果目标元素不是其父元素的最后一个元素，则把新元素放在和目标元素相邻的下一个兄弟元素的前面
    parent.insertBefore(newElement,targetElement.nextSibling);
  }
}
```