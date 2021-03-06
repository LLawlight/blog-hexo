---
title: "this"
date: 2016-05-11
categories: 
- 技术
- JavaScript
tags: 
- 技术
- JavaScript
- this
---

## this的指向

### 1.作为对象的方法调用

当函数作为对象的方法被调用是，this指向该对象：
```js
var obj = {
  a: 1,
  getA: function() {
    alert ( this === obj );	//输出：true
    alert ( this.a );	//输出： 1
  }
};
obj.getA();
```

<!-- more -->

### 2.作为普通函数调用

此时的this总是指向全局对象。在浏览器的JavaScript里，这个全局对象是window对象。
```js
window.name = 'globalName';

var getName = function(){
  return this.name;
};

console.log( getName() );	//输出：globalName
```
或者：
```js
window.name = 'globalName';

var myObject = {
  name: 'sven',
  getName: function() {
    return this.name;
  }
};

var getName = myObject.getName;
console.log( getName() );	//globalName
```
比如在div节点的事件函数内部，有一个局部的callback方法，callback被作为普通函数调用时，callback内部的this指向了window，但我们往往是想让它指向该div节点，见如下代码：
```html
<html>
  <body>
    <div id="div1">我是一个div</div>
  </body>
  <script>
  window.id = 'window';
  document.getElementById( 'div1' ).onclick = function() {
    alert( this.id );	//输出： 'div1'
    var callback = function() {
      alert( this.id );	//输出：'window'
    }
  callback();
  };
</script>
</html>
```
此时有一种简单的解决方案，可以用一个变量保存div节点的引用：
```js
document.getElementById( 'div1' ).onclick = function() {
  var that = this;	//保存div的引用
  var callback = function() {
    alert( that.id );	//输出：'div1'
  }
  callback();
};
```
在ECMAScript5的strict模式下，这种情况下的this已经被规定为不会指向全局对象，而是undefined：
```js
function func() {
  "use strict"
  alert ( this );	//输出：undefined
}
func();
```
### 3.构造器调用

当用new运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的this就指向返回的这个对象。
```js
var myClass = function() {
  this.name = 'sven';
};

var obj = new myClass();
alert ( obj.name );	//输出：sven
```
但用new调用构造器时，还要注意一个问题，如果构造器显式地返回了一个object类型的对象，那么此次运算结果最终会返回这个对象。
```js
var myClass = function() {
  this.name = 'sven';
  return {	//显式地返回一个对象
    name: 'anne'
  }
};

var obj = new myClass();
alert ( obj.name );	//输出：anne
```
如果构造器不显示地返回任何数据，或者是返回一个非对象类型的数据，就不会造成上述问题：
```js
var myClass = function() {
  this.name = 'sven';
  name = 'anne';
  return name;	//返回string类型
};

var obj = new myClass();
alert ( obj.name );	//输出：sven
```
### 4.Function.prototype.call或Function.prototype.apply调用

用Function.prototype.call或Function.prototype.apply可以动态地改变传入函数的this：
```js
var obj1 = {
  name: 'sven',
  getName: function() {
    return this.name;
  }
};
var obj2 = {
  name: 'anne'
};
console.log(obj1.getName());	//输出： sven
console.log(obj1.getName.call(obj2));	//输出： anne
```
## 丢失的this

document.getElementById这个方法名过长，我们尝试用一个短的函数代替它：
```js
var getId = function( id ) {
  return document.getElementById( id );
};
getId( 'div1' );
```
我们也许思考过为什么不能用下面这种更简单的方式：
```js
var getId = document.getElementById;
getId( 'div1' );
```
在浏览器中运行下列代码：
```html
<html>
  <body>
    <div id="div1">我是一个div</div>
  </body>
  <script>
    var getId = document.getElementById;
    getId( 'div1' );
  </script>
</html>
```
发现这段代码抛出了一个异常。这是因为许多引擎的document.getElementById方法的内部实现中需要用到this。这个this本来被期望指向document，当getElementById方法作为document对象的属性被调用时，方法内部的this确实是指向document的。

但当用getId来引用document.getElementById之后，在调用getId，此时就成了普通函数调用，函数内部的this指向了window，而不是原来的document。

我们可以尝试利用apply把document当作this传入getId函数，帮助“修正”this：
```js
document.getElementById = (function( func ) {
  return function() {
    return func.apply( document, arguments );
  }
})(document.getElementById);

var getId = document.getElementById;
var div = getId( 'div1');

alert(div.id);	//输出：div1
```
---
参考资料：《JavaScript设计模式与开发实践》