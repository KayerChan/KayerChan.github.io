---
layout: post
title: 解决ng-bind-html 没有样式的问题
date: 2018-06-06
categories: blog
tags: [ng-bind,angular]
description: 纯技术分享。
---
## 解决ng-bind-html 没有样式的问题
html标签中加入过滤器，过滤器引入$sce内置的服务
```
app.filter('html_trust',['$sce',function ($sce) {
    return function (text) {
        return $sce.trustAsHtml(text);
    }
}])
```
