---
title: 解决Argon主题预览时只能点击标题进入文章
tags:
  - Argon
  - html
id: '835'
categories:
  - - 学习笔记
  - - 文章
date: 2022-04-04 18:20:46
---

###### 找到自己相对应文章列表卡片布局。比如我的为布局1

![image-20220404181846323](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220404181846323.png)

###### 进入不同的preview.php文件。

![image-20220404181432492](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220404181432492.png)

###### 找到 class="post-content", 在后面添加：

```js
onclick="window.location.href='<?php the_permalink(); ?>'" style="cursor: pointer"
```