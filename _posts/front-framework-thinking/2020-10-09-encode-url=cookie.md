---
title: "URL编码与cookie"
subtitle: ""
layout: post
author: "libyasdf"
header-style: text
tags:
  - encode
  - url编码
---
[为什么要URL编码](https://www.cnblogs.com/jerrysion/p/5522673.html)  

### js 三对函数用来对Url编码
 escape / unescape  

 encodeURI / decodeURI  

 encodeURIComponent / decodeURIComponent  

下面列出了这三个函数的安全字符（即函数不会对这些字符进行编码）

>escape（69个）：*/@+-._0-9a-zA-Z  
encodeURI（82个）：!#$&'()*+,/:;=?@-._~0-9a-zA-Z  
encodeURIComponent（71个）：!'()*-._~0-9a-zA-Z  

**encodeURI**和**encodeURIComponent**则使用UTF-8对非ASCII字符进行编码，然后再进行百分号编码。这是RFC推荐的。因此建议尽可能的使用这两个函数替代escape进行编码。

### 表单提交
当Html的表单被提交时，每个表单域都会被Url编码之后才在被发送。由于历史的原因，表单使用的Url编码实现并不符合最新的标准。

例如对于空格使用的编码并不是%20，而是+号，如果表单使用的是Post方法提交的，我们可以在HTTP头中看到有一个Content-Type的header，值为:
```
application/x-www-form-urlencoded
```

#### 客户端存储-红宝书25
