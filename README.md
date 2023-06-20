<!--
 * @Descripttion: 
 * @version: 
 * @Author: 王远昭
 * @Date: 2023-06-21 00:56:11
 * @LastEditors: 王远昭
 * @LastEditTime: 2023-06-21 00:56:23
-->
新建文章，在blog\source_posts

```
hexo new post (文章名称)
```

新建页面，在blog\source

```
hexo new page (页面名称)
```

清除缓存文件 (db.json) 和已生成的静态文件 (public)

```
hexo clean
```

生成静态文件

```
hexo g
```

启动服务器，访问网址:[http://localhost:4000](http://localhost:4000/)

```
hexo s
```

部署网站

```
hexo d
```

hexo三连,四连

```
hexo cl && hexo g && hexo s
hexo cl && hexo g && gulp && hexo d
```

push三连

```
git add .
git commit -m "github action update"
git push origin master
```