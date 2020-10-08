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

#### NOTE:
package.json中的:
```
"homepage": "https://libyasdf.github.io/data-structure-docs",
```
是部署后加载到资源的关键。

>homepage 的作用是设置应用的跟路径，我们的项目打包后是要运行在一个域名之下的，有时候可能是运行在跟域名下，也有可能运行在某个子域名下或或域名的某个目录下，这时候我们就需要让我们的应用知道去哪里加载资源，这时候就需要我们设置一个跟路径，而且有时候我们的资源会部署在 CDN 上，你必须告诉打包工具你的CDN地址是什么。

[homepage属性](https://blog.csdn.net/duninet/article/details/104537393)  
