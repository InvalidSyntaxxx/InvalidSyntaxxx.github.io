---
title: WordPress完整建站过程(一)
tags:
  - Apache
  - MySQL
  - PHP
  - WordPress
id: '304'
categories:
  - - 学习笔记
date: 2022-03-02 18:13:41
---

我的👉[第一篇文章](https://wangwangyz.site/?p=213 "第一篇文章")👈里面就写了些建站的经过，现在我打算把过程记录下来，也算是一种复习吧（是因为孩怕哪天服务器过期了要重新搭建。。。）

## 前期准备

1.  一台有公网IP的主机（我的是👉[阿里云](https://www.aliyun.com/ "阿里云")轻量服务器）
2.  域名以及DNS解析
3.  wordpress安装包（👉[官网下载](https://cn.wordpress.org/latest-zh_CN.zip "官网下载")，👉[站长自存](http://oos.wangwangyz.site/wordpress%E4%B8%BB%E9%A2%98%E5%8C%85/wordpress-5.9-zh_CN.zip "站长自存")）
<!-- more -->
## 开始安装

1.  云服务器最好先选择Ubuntu20.04环境（虽然云服务器上有wordpress环境和LAMP、LNMP环境，但还是想自己折腾）
2.  域名解析(可选) 如何将域名解析到自己主机上的方法自行查找，先有了主机再买域名弄比较合适
3.  在云服务器上安装LAMP或者LNMP LNMP是指Linux、Ngnix、MySql、PHP环境 LAMP是指Linux、Apache、MySql、PHP环境（我这里使用的是这个）
    
    *   先更新一下云服务器Linux（为了获得系统当前最新的软件包）
        
        ```cpp
        sudo apt-get update
        sudo apt-get upgrade
        ```
        
    *   安装Apache2(用来做wordpress的服务器)
        
        ```cpp
        sudo apt-get install apache2
        ```
        
    *   安装MySQL(用来存放网站的所有数据)
        
        1.  安装mysql-server
        
        ```
        sudo apt-get install mysql-server
        ```
        
    
    2.  初始化设置
        
        ```
        sudo mysql_secure_installation
        ```
        
    3.  检查mysql服务状态
        
        ```
        systemctl status mysql.service
        ```
        
    4.  配置远程连接
        
        ```
        sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf 
        #找到 bind-address 修改值为 0.0.0.0(如果需要远程访问)
        ```
        
    5.  重启mysql
        
        ```
        sudo /etc/init.d/mysql restart 
        ```
        
    6.  修改密码
        
        ```sql
        >>use mysql;
        >>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码'; 
        >>flush privileges;
        >>quit;
        ```
        
    7.  配置所有ip都可访问(否则后续访问PHPmyadmin会出现权限问题)
        
        ```sql
        UPDATE user SET host = '%' WHERE user = 'root'; #允许远程访问
        ```
        
    
    *   安装PHP(网页的后端语言)
        
        ```c
        sudo apt-get install php
        ```
        
    *   安装PHP-MySQL（用于PHP和MySQL之间的支持） \`\`\`c sudo apt-get install php-mysql <pre><code>-安装PHPmyadmin(用于在网页上管理mysql数据库) \`\`\`c sudo apt-get install phpmyadmin
4.  由于phpmyadmin的文件在usr/share/phpmyadmin中，而网页访问的根目录在www/html中，需要在根目录创建快捷方式：
    
    ```
    sudo ln -s /usr/share/phpmyadmin /var/www
    ```
    
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
        
    *   网页上访问localhost/phpmyadim,用mysql数据库root/密码登陆 ![](https://gitee.com/wang-yuanzhao/personal-drawing-bed/raw/master/images/image-20220310115040831.png)
        
    *   出现这类错误先查看是不是phpmyadmin的用户名和密码输错了![image-20220310115531052](https://gitee.com/wang-yuanzhao/personal-drawing-bed/raw/master/images/image-20220310115531052.png)
        
        如何查看phpmyadmin的登陆用户名和密码
        
    
    ```
    vim /etc/phpmyadmin/config-db.php
    ```
    
    ![image-20220310120720517](https://gitee.com/wang-yuanzhao/personal-drawing-bed/raw/master/images/image-20220310120720517.png) 再重新登录即可 登陆成功
    
    ![image-20220310121208518](https://gitee.com/wang-yuanzhao/personal-drawing-bed/raw/master/images/image-20220310121208518.png) (为什么不用宝塔面板之前那篇👉[文章](https://wangwangyz.site/?p=213 "文章")有介绍)
    
    ### [WordPress完整建站过程(二)](https://wangwangyz.site/?p=562 " WordPress完整建站过程(二)")