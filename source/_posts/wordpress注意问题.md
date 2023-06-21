---
title: 遇到的Wordpress问题集
tags:
  - MySQL
  - PHP
  - WordPress
id: '328'
categories:
  - - 学习笔记
  - - 文章
date: 2022-03-01 10:10:48
---

在网站的搭建过程中难免会遇到一些问题，本篇文章将过程记录下来，分享给大家，希望对大家有帮助。各位遇到的问题也欢迎留言讨论，一起学习~ 问题链接:

*   ##### [Wordpress头像显示问题](https://wangwangyz.site/archives/837)
    
*   ##### [解决Argon主题预览时只能点击标题进入文章](https://wangwangyz.site/archives/835)
    
*   ##### [Wordpress伪静态链接问题](https://wangwangyz.site/archives/919)
    
<!-- more -->
一些小问题:

#### 1、Wordpress5.9要安装Markdown编辑器的话用 `WP-githuber md`

首先要先禁用新版编辑器 `古堡疼`，在主题文件编辑器那里找到 function.php，在最后添加上

```php
//禁用古腾堡编辑器
add_filter(&#039;use_block_editor_for_post&#039;, &#039;__return_false&#039;);
//屏蔽古腾堡的样式加载
remove_action( &#039;wp_enqueue_scripts&#039;, &#039;wp_common_block_scripts_and_styles&#039; );
```

Wordpress插件推荐：`WPvivid Backup Plugin`(备份插件)、`WP Statistics`(统计插件，PC端右侧的统计信息就是使用该插件)、`WP Githuber MD` （Markdown语法支持插件）

#### 2、后台修改WordPress Address（URL）后导致无法登陆后台的解决办法

登陆数据数据库，进入wordpress数据库

```sql
sudo mysql -u root -p
use wordpress;
update wp_options set option_value="http://xx.xx.xx.xx" where option_name="siteurl";
update wp_options set option_value="http://xx.xx.xx.xx" where option_name="home";
```

**option\_value要设置回原服务器的ip/域名，注意要加协议名http或https**(根据网站是否配置了https证书选择)

#### 3、关于phpmyadmin出现404原因

> 环境：Ubuntu20.04 Apache2.4

一、由于phpmyadmin的文件在usr/share/phpmyadmin中，而网页访问的根目录在www/html中，需要在根目录创建快捷方式 ：`sudo ln -s /usr/share/phpmyadmin /var/www`

*   查看文件
    
    ```
    sudo gedit /etc/apache2/apache2.conf
    ```
    
*   进入文本编译器之后，会看到很长的代码 在末尾加上这句话:
    
    ```
    Include /etc/phpmyadmin/apache.conf
    ```
    
*   保存退出重启apache
    
    ```
    /etc/init.d/apache2 restart
    ```
    
*   访问localhost/phpmyadim,用mysql数据库root/密码登陆
    

#### 4、解决配置CDN后台IP定位不准确

> wordpress版本：5.9.2

在根目录下找到 `wp-config.php`,在最后给它添加上：

```php
if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
 $list=explode(',',$_SERVER['HTTP_X_FORWARDED_FOR']);
 $_SERVER['REMOTE_ADDR'] = $list[0];
}
```