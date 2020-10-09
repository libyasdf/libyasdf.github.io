---
title: "javascript 中的 class static"
subtitle: ""
layout: post
author: "libyasdf"
header-style: text
tags:
  - static
  - class
---
## 静态方法
>类相当于实例的原型， 所有在类中定义的方法， 都会被实例继承。 如果在一个方法前， 加上static关键字， 就表示该方法不会被实例继承， 而是直接通过类来调用， 这就称为“ 静态方法”。
[static class](https://www.jb51.net/article/127066.htm)  

## 静态属性

>静态属性指的是 Class 本身的属性， 即Class.propname， 而不是定义在实例对象（ this） 上的属性。

```
class Foo {}
Foo.prop = 1;
Foo.prop // 1
```
**目前， 只有这种写法可行， 因为 ES6 明确规定， Class 内部只有静态方法， 没有静态属性。**

## ES7 静态属性的提案
*目前 Babel 转码器支持*
### 类的实例属性
类的实例属性可以用等式， 写入类的定义之中
```
class MyClass {
  myProp = 42;
  constructor() {
    console.log(this.myProp); // 42
  }
}
```
>以前， 我们定义实例属性， 只能写在类的constructor方法里面。
```
class ReactCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
}
```
>新的写法以后， 可以不在constructor方法里面定义
```
class ReactCounter extends React.Component {
  state = {
    count: 0
  };
}
```
>为了可读性的目的， 对于那些在constructor里面已经定义的实例属性， 新写法允许直接列出:
```
class ReactCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
  state;
}
```
### 类的静态属性

>类的静态属性只要在上面的实例属性写法前面， 加上static关键字就可以了。
```
class MyClass {
static myStaticProp = 42;
constructor() {
console.log(MyClass.myProp); // 42
}
}
```
>新写法
```
// 老写法
class Foo {}
Foo.prop = 1;
// 新写法
class Foo {
  static prop = 1;
}
```