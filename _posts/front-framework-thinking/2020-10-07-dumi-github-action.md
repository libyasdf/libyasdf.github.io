---
title: "dumi 与GitHub action"
subtitle: "blog制作与GitHub action的自动部署"
layout: post
author: "libyasdf"
header-style: text
tags:
  - action
  - dumi
---
[代码GitHub地址](https://github.com/libyasdf/data-structure-docs)  

# dumi
[dumi官网](https://d.umijs.org/config/frontmatter)  
有三种搭建方式可以开始blog<br/>
需要注意：
文件夹嵌套的方式，决定了左侧菜单栏与导航栏的显示

# github action
1、从全局*setting/developer settings/personal access token*获取到token  
2、然后从项目的setting中，设置**Secrets**，取好名字。
3、将取好的名字，在GitHub workflow action .yml文件中，使用<br/>

```
${{ secrets.ACCESS_TOKEN }}
```
4、token的权限与部署的成功休戚相关。

# 比较Jekyll与dumi

github天然支持Jekyll，在项目[ts-vue-cli](https://github.com/libyasdf/ts-vue-cli)中，使用Jekyll搭建的文档，在GitHub pages指向gh-pages/docs时，如同在本地执行:
```
bundle exce jekyll serve
```
的效果。

反观dumi，使用GitHub action，随着push，而部署。
