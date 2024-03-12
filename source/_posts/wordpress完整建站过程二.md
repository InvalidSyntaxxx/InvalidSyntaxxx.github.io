---
title: wordpress完整建站过程(二)
tags:
  - Apache
  - Linux
  - MySQL
  - PHP
  - WordPress
id: '562'
categories:
  - - 学习笔记
  - - 文章
date: 2022-03-10 12:17:02
---

1.  通过FTP软件将下载的wordpress包传输到Linux的/var/www/html/目录下 ![通过FTP软件将下载的wordpress包传输到Linux](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/%E9%80%9A%E8%BF%87FTP%E8%BD%AF%E4%BB%B6%E5%B0%86%E4%B8%8B%E8%BD%BD%E7%9A%84wordpress%E5%8C%85%E4%BC%A0%E8%BE%93%E5%88%B0Linux.png) 或者通过wget获取我站点上的安装包（看情况万一我的OSS服务没了这个方法也就不行了） 在当前根目录获取安装包 `sudo wget https://redamancy9189.oss-cn-beijing.aliyuncs.com/wordpress%E4%B8%BB%E9%A2%98%E5%8C%85/wordpress-5.9-zh_CN.zip` 解压 `unzip wordpress-5.9-zh_CN.zip` 移动到根目录 `mv wordpress /var/www/` 删除原本的html并修改wordpress为html `cd /var/www` `rm -rf html` `mv wordpress html`
2.  打开服务器站点（一般为locahost）,根据提示输入数据库密码并进行安装 ![image-20220310130420800](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220310160319591.png)
3.  完成安装，进入后台页面 ![Wordpress界面](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/Wordpress%E7%95%8C%E9%9D%A2.png) 4.安装主题，开始编写自己的文章吧！
<!-- more -->
### 解决wordpress安装主题时的问题

*   解决安装WordPress主题及插件需要输入FTP问题
    
    安装一个WordPress好像挺简单，但是默认主题不喜欢，想更换一个，无奈本地可以更换，但是服务器更换的时候需要设置FTP 。OK，设置呗，好像我的用户名密码之类的都是正确的，就是不让我通过，因此，找了一下解决方案
    
    进入WordPress根目录
    
    ```javascript
    vim wp-config.php
    ```
    
    添加以下三句代码
    
    ```javascript
    define("FS_METHOD", "direct");
    define("FS_CHMOD_DIR", 0777);
    define("FS_CHMOD_FILE", 0777);
    ```
    
    接着重新访问你的网站，重新安装主题
    
*   无法建立目录wp-content/uploads/xxxx/xx。有没有上级目录的写权限？
    
    给wp-content目录添加权限 `sudo chmod -R 777 wordpress/wp-content`
    
*   Nginx出现 `413Request Entity Too Large` 打开nginx.conf配置文件 `sudo vim /etc/nginx/nginx.conf`
    

在http{}中加入`client_max_body_size 100M`; 重启ngnix；

#### Wordpress5.9要安装Markdown编辑器的话用 `WP-githuber md`

首先要先禁用新版编辑器 `古堡疼`，在主题文件编辑器那里找到 function.php，在最后添加上

```php
//禁用古腾堡编辑器
add_filter('use_block_editor_for_post', '__return_false');
//屏蔽古腾堡的样式加载
remove_action( 'wp_enqueue_scripts', 'wp_common_block_scripts_and_styles' );
```

Wordpress插件推荐：`WPvivid Backup Plugin`、`WP Statistics`、`WP Githuber MD`

![](https://gitee.com/wang-yuanzhao/personal-drawing-bed/raw/master/images/image-20220302200717627.png)