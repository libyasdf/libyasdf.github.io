---
title: "前端项目搭建总结"
subtitle: "前端项目构建经验记录"
layout: post
author: "libyasdf"
header-style: text
tags:
  - JavaScript
  - 框架
  - React
---

# 前端项目开发思考

思考点:

* 项目结构：微前端架构
* UI库
* 状态管理
* 前后端交互
* 常用场景组件封装
* 表单
* 路由
* 样式管理
* 浏览器支持
* 脚手架

## 项目结构：微前端架构

项目现在面临的问题：将新老项目集成在一起，呈现给客户。

未来面临的问题：大型前端项目的维护与扩展弊端。这个与大型后端项目是一样的。现在项目组想将后端拆分多个项目，同理，前端也是一样的。

所以，需要有微前端架构。将前端项目按照业务模块进行拆分，然后通过某些技术、工具或者框架，将项目最终组合在一起，呈现给客户。

每个微前端项目都可以独立开发、测试、部署，但是最终合成一个整体，呈现给客户试用。

推荐采用[umi的乾坤](https://github.com/umijs/qiankun)。

采用gitlab做源码管理工具。

将整个前端项目放在一个大仓库中管理。这样更容易做公共包的引用。结构如下：

```
天津前端项目
|__ commons
	|__ use-current-user （获取当前登录用户信息的hook）
|__ packages
	|__ 项目1
	|__ 项目2
	|__ 项目3
	|__ 项目4
```

* `commons` - 存放公共模块的地方
* `packages` - 存放各个微前端项目的地方

每个项目可以采用create-react-app创建初始化模板。

采用VSCode。不要用vscode整体打开前端项目，而是以`package`中的项目为单位打开。

不建议采用yarn workspace。

### 子项目结构

项目组自己整理？需要考量什么，结构基本上就是什么样的。

### 依赖管理

建议保证所有项目中的依赖库版本尽可能保持一致。如果确实有不一致的情况。需要在根目录的`README.md`中写清楚原因。依赖管理应该从一开始就考虑进来。在不具备自动化能力前，手动管理这些依赖关系。

## UI库

目前自研UI库还在建设中（@sinoui/core），不建议项目组目前采纳。目前推荐项目采纳ant design。

后期，@sinoui/core发布了1.0.0版本后，会推荐给项目组使用。

## 状态管理

状态管理上，多采用组件级的局部状态。全局状态采用Redux。

原则：如无必要，不采用全局状态，采用局部状态。

遵循：

* 状态唯一性

### 全局状态

需要分辨出哪些是全局状态，哪些是局部状态。全局状态采用Redux管理。局部状态采用组件状态管理。

全局状态：

* 当前用户信息
* ...（一时半会想不出来）

关于全局状态的获取，需要封装成共用hook，例如：

获取当前用户：

```ts
import userCurrentUser from '@commons/use-current-user';

const user = useCurrentUser();
```

全局状态共享还有一个前提，就是跨越微前端的鸿沟。将这些均封装到`useCurrentUser`中去。

### 局部状态

局部状态采用React组件状态即可。推荐项目组采用函数组件的hook管理局部状态。

对于组件层级比较深的，采用Context传递。

### 复杂页面交互的状态管理

对于复杂页面交互，状态管理也往往比较复杂，页面的不同部分可能会有状态共享、状态交互等情况，那么需要使用“重武器”，如：

* 页面级的Redux（注意，不是全局的Redux）
* unstated-next

## 前后端交互

前后端交互采用REST风格的接口。

前端可以采用[@sinoui/http](https://github.com/sinoui/http)这个Ajax库来与后端进行交互。

采用异步函数语法和Promise接口，来处理异步动作。不要使用回调函数形式的。

## 常用场景组件封装

建议项目组将常用组件封装安排下去。由一个专业级的前端做组件设计，分配给其它开发人员实现（不管前端还是后端开发人员，后端开发人员更得多分配此类开发任务，因为一般常用组件，设计部分新手不太容易把控住，但是由专业前端设计出来后，再去实现，是比较简单的，甚至比开发一些稍微复杂一点点的页面都要简单。当然，让组件长得好看除外。写样式的部分，还是可以交给专业前端或者擅长样式的同事写。还好React是组件化的，所以能够将UI与逻辑组件分配开来的。）一定要这么做，要不然有两个后果：

* 稀少的前端同事到后期会累死
* 其它开发同事永远是个门外汉（对于前端技术）

我能想到的常用组件有：

* 流程提交
* 流程意见
* 增删改查相关的组件
  * 列表（可以基于antd的列表组件，稍微做一些轻量封装，或者直接用antd的列表组件，感觉有重复代码时，再着手做封装）
  * 表单（使用antd的表单组件即可。后面章节有介绍）
  * 
* 页面布局
* loading
* 人员部门树选择组件

可以一块看看页面设计，或者看看以前的系统，一块找找，看看哪些部分可以用现成的组件，哪些部分可能需要自己封装。

自己封装的，大部分是带有通用业务逻辑的组件。

这些封装的组件，均放到`commons`目录中。

一个组件一个小模块。

还可以将一些常用逻辑封装成hook，例如：

* 基于Rest风格的增删改查API交互：
  * [@sinoui/use-rest-item-api](http://github.com/sinoui/use-rest-item-api)
  * [@sinoui/use-rest-page-api](https://github.com/sinoui/use-rest-page-api)
* ...（一时半会想不到）

**公共组件一定要写storybook，单独验证，不要在项目中整体去验证。**

## 表单

antd的表单能够应付日常开发的需求。

对于复杂场景的，可以使用[@sinoui/rx-core-form](https://github.com/sinoui/rx-core-form)。（需要用到的时候，可以找平台组刘进行讨论。）

## 路由

路由管理，一般采用React-Router。记得需要采用异步加载路由组件（即页面）。

## 样式管理

建议采用[styled-components](https://www.styled-components.com/)封装组件样式。

## 浏览器支持

建议直接采用IE11和其它浏览器。有条件的，就直接采用谷歌浏览器。

React、Vue这类项目是重JS项目，特别是做中后台项目，js代码量是比较多的。IE11以下浏览器一般会遇到卡顿的情况。

## 脚手架

微前端采用[umi的乾坤](https://github.com/umijs/qiankun)。单个项目采用create-react-app即可。公共库，建议采用typescript编写，这样使用公共组件时，有接口提示。

公共库的脚手架或者模板由项目组自己整理？

* typescript
* eslint
* storybook