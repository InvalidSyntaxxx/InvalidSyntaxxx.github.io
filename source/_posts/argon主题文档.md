---
title: Argon主题文档
tags: []
id: '618'
categories:
  - - 专业技术
date: 2022-03-13 21:01:38
---

文档地址：[https://argon-docs.solstice23.top/#/](https://argon-docs.solstice23.top/#/)

### 1、图标

以下代码能实现下图中的效果

```c
<i class="fa fa-home"></i> 首页
<i class="fa fa-link"></i> 友链
<i class="fa fa-rss"></i> RSS
```

首页 友链 RSS
<!-- more -->
请访问 [Font Awesome 4](https://fontawesome.com/v4/icons/ "Font Awesome 4 ") 图标库自行查询对应的图标名。

### 2、标签

`[label 参数名="参数值"]内容[/label]`

```c
方形
[label]默认标签[/label]
[label color="indigo"]靛蓝标签[/label]
[label color="blue"]蓝色标签[/label]
[label color="orange"]橙色标签[/label]
圆形
[label color="indigo" shape="round"]靛蓝标签[/label]
[label color="blue" shape="round"]蓝色标签[/label]
[label color="orange" shape="round"]橙色标签[/label]
```

方形 \[label\]默认标签\[/label\] \[label color="indigo"\]靛蓝标签\[/label\] \[label color="blue"\]蓝色标签\[/label\] \[label color="orange"\]橙色标签\[/label\] 圆形 \[label color="indigo" shape="round"\]靛蓝标签\[/label\] \[label color="blue" shape="round"\]蓝色标签\[/label\] \[label color="orange" shape="round"\]橙色标签\[/label\]

### 3、进度条

`[progressbar 参数名="参数值"]进度条标签内容[/progressbar]` 标签内容可以不填写

```c
[progressbar progress="20"]默认进度条[/progressbar]
[progressbar progress="30" color="indigo"]靛蓝进度条[/progressbar]
[progressbar progress="40" color="green"]绿色进度条[/progressbar]
[progressbar progress="66" color="red"]红色进度条[/progressbar]
[progressbar progress="80" color="blue"]蓝色进度条[/progressbar]
[progressbar progress="100" color="orange"]橙色进度条[/progressbar] 
[progressbar progress="23"][/progressbar]
[progressbar]没有指明参数的进度条[/progressbar]
[progressbar progress="66.66"]小数进度条[/progressbar]
```

\[progressbar progress="20"\]默认进度条\[/progressbar\] \[progressbar progress="30" color="indigo"\]靛蓝进度条\[/progressbar\] \[progressbar progress="40" color="green"\]绿色进度条\[/progressbar\] \[progressbar progress="66" color="red"\]红色进度条\[/progressbar\] \[progressbar progress="80" color="blue"\]蓝色进度条\[/progressbar\] \[progressbar progress="100" color="orange"\]橙色进度条\[/progressbar\] \[progressbar progress="23"\]\[/progressbar\] \[progressbar\]没有指明参数的进度条\[/progressbar\] \[progressbar progress="66.66"\]小数进度条\[/progressbar\]

### 4、警告

`[admonition 参数名="参数值"]内容[/admonition]`

```c
[admonition]默认警告[/admonition]
[admonition title="我是标题" color="indigo"]靛蓝警告[/admonition]
[admonition title="我是标题" color="green"]绿色警告[/admonition]
[admonition title="我是标题" color="red"]红色警告[/admonition]
[admonition title="我是标题" color="blue"]蓝色警告[/admonition]
[admonition title="我是标题" color="orange"]橙色警告[/admonition]
[admonition title="我是标题" color="black"]黑色警告[/admonition]
[admonition title="我是标题" color="grey"]灰色警告[/admonition]
[admonition title="我是标题" icon="flag" color="indigo"]带标题和图标的警告[/admonition]
[admonition color="indigo"]不带标题的警告[/admonition]
[admonition title="只有标题的警告" color="indigo"][/admonition]
[admonition title="只有标题和图标的警告" icon="flag" color="indigo"][/admonition]
```

\[admonition\]默认警告\[/admonition\] \[admonition title="我是标题" color="indigo"\]靛蓝警告\[/admonition\] \[admonition title="我是标题" color="green"\]绿色警告\[/admonition\] \[admonition title="我是标题" color="red"\]红色警告\[/admonition\] \[admonition title="我是标题" color="blue"\]蓝色警告\[/admonition\] \[admonition title="我是标题" color="orange"\]橙色警告\[/admonition\] \[admonition title="我是标题" color="black"\]黑色警告\[/admonition\] \[admonition title="我是标题" color="grey"\]灰色警告\[/admonition\] \[admonition title="我是标题" icon="flag" color="indigo"\]带标题和图标的警告\[/admonition\] \[admonition color="indigo"\]不带标题的警告\[/admonition\] \[admonition title="只有标题的警告" color="indigo"\]\[/admonition\] \[admonition title="只有标题和图标的警告" icon="flag" color="indigo"\]\[/admonition\]

### 5、Github信息卡

`[github 参数名="参数值"][/github]`

```c
 [github author="solstice23" project="argon-theme"][/github]
```

\[github author="solstice23" project="argon-theme"\]\[/github\]

参数名

可选值

默认

解释

是否必须

author

字符串

空

Repo作者名

是

color

字符串

空

Repo名

是

size

full/mini

full

尺寸

否

getdata

frontend/backend

前端/后端获取Reop信息

否