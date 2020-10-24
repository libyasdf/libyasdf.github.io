---
title: "作用域、原型链、闭包"
subtitle: ""
layout: post
author: "libyasdf"
header-style: text
tags:
  - 作用域
  - prototype chain
  - 闭包
---
[基本知识](https://juejin.im/post/6844904160719011848#heading-10)  

# 作用域

* 在JavaScript中，通过 let 和 const 定义的变量具有块级作用域的特性。  
* 通过 var 定义的变量会在它自身的作用域中进行提升，而 let 和 const 定义的变量不会。
* 每个JavaScript程序都具有一个全局作用域，每创建一个函数都会创建一个作用域。  
* 在创建函数时，将这些函数进行嵌套，它们的作用域也会嵌套，形成作用域链，子作用域可以访问父作用域，但是父作用域不能访问子作用域。  
* 在执行一个函数时，如果我们**需要查找某个变量值，那么会去这个函数被 定义 时所在的作用域链中查找，一旦找到需要的变量，就会停止向上查找。** 
* “变量的值由函数定义时的位置决定”这句话有歧义，准确说是查找变量时，是去定义这个函数时所在的作用域链查找。  

# 原型链
## this
ES5中:   
* this 永远指向最后调用它的那个对象  

ES6箭头函数:  
* 箭头函数的 this 始终指向函数定义时的 this，而非执行时。

改变this的指向:  

* 使用 ES6 的箭头函数  
* 在函数内部使用 _this = this  
* 使用 apply、call、bind  
* new 实例化一个对象  

## 箭头函数
没有this  
没有arguments  
不能通过new关键字调用  
没有new.target  
没有原型  
没有super  

## constructor prototype __proto__
[小猪例子](https://juejin.im/post/6844903604575272974)  

>每一个函数都会有prototype属性  
>每一个原型都会有一个constructor  
>每一个实例/对象都会有一个__proto__指针  

![原型](/img/html/prototype.png)  

# 闭包
让外部访问函数内部变量成为可能；

局部变量会常驻在内存中；

可以避免使用全局变量，防止全局变量污染；

会造成内存泄漏（有一块内存空间被长期占用，而不被释放）

**闭包找到的是同一地址中父级函数中对应变量最终的值**
>闭包指的是能够访问另一个函数作用域的变量的函数。闭包就是一个函数，这个函数能够访问其他函数的作用域中的变量。

应用：[函数防抖](https://segmentfault.com/a/1190000018428170) 封装私有变量

**对于短时间内连续触发的事件（上面的滚动事件），防抖的含义就是让某个时间期限（如上面的1000毫秒）内，事件处理函数只执行一次。**
```
function debounce(fn,delay){
    let timer = null //借助闭包
    return function() {
        if(timer){
            clearTimeout(timer) 
        }
        timer = setTimeout(fn,delay) // 简化写法
    }
}
// 然后是旧代码
function showTop  () {
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
　　console.log('滚动条位置：' + scrollTop);
}
window.onscroll = debounce(showTop,1000) // 为了方便观察效果我们取个大点的间断值，实际使用根据需要来配置
```

[闭包函数例子](https://blog.csdn.net/weixin_43586120/article/details/89456183)  

```
var Counter = (function() {
  // 封装私有变量
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }   
})();

console.log(Counter.value()); /* logs 0 */
Counter.increment();
Counter.increment();
console.log(Counter.value()); /* logs 2 */
Counter.decrement();
console.log(Counter.value()); /* logs 1 */
```
>[作者：寸志](https://www.zhihu.com/question/34210214/answer/93590294)  
JavaScript 闭包的本质源自两点，词法作用域和函数当作值传递。

>词法作用域，就是，按照代码书写时的样子，内部函数可以访问函数外面的变量。引擎通过数据结构和算法表示一个函数，使得在代码解释执行时按照词法作用域的规则，可以访问外围的变量，这些变量就登记在相应的数据结构中。函数当作值传递，即所谓的first class对象。就是可以把函数当作一个值来赋值，当作参数传给别的函数，也可以把函数当作一个值 return。一个函数被当作值返回时，也就相当于返回了一个通道，这个通道可以访问这个函数词法作用域中的变量，即函数所需要的数据结构保存了下来，数据结构中的值在外层函数执行时创建，外层函数执行完毕时理因销毁，但由于内部函数作为值返回出去，这些值得以保存下来。而且无法直接访问，必须通过返回的函数。这也就是私有性。本来执行过程和词法作用域是封闭的，这种返回的函数就好比是一个虫洞，开了挂。也就是 @秋月凉 答案中轮子哥那句话的意思。显然，闭包的形成很简单，在执行过程完毕后，返回函数，或者将函数得以保留下来，即形成闭包。实际上在 JavaScript 代码中闭包不要太常见。函数作为第一等对象之后 JavaScript 灵活得不要不要的。

# 节流

*区别*如果在限定时间段内，不断触发滚动事件（比如某个用户闲着无聊，按住滚动不断的拖来拖去），只要不停止触发，理论上就永远不会输出当前距离顶部的距离。

>类似控制阀门一样定期开放的函数，也就是让函数执行一次后，在某个时间段内暂时失效，过了这段时间后再重新激活（类似于技能冷却时间）。

```
function throttle(fn,delay){
    let valid = true
    return function() {
       if(!valid){
           //休息时间 暂不接客
           return false 
       }
       // 工作时间，执行函数并且在间隔期内把状态位设为无效
        valid = false
        setTimeout(() => {
            fn()
            valid = true;
        }, delay)
    }
}
/* 请注意，节流函数并不止上面这种实现方案,
   例如可以完全不借助setTimeout，可以把状态位换成时间戳，然后利用时间戳差值是否大于指定间隔时间来做判定。
   也可以直接将setTimeout的返回的标记当做判断条件-判断当前定时器是否存在，如果存在表示还在冷却，并且在执行fn之后消除定时器表示激活，原理都一样
    */

// 以下照旧
function showTop  () {
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
　　console.log('滚动条位置：' + scrollTop);
}
window.onscroll = throttle(showTop,1000) 
```