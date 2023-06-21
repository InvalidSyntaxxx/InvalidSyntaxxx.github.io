---
title: 设置Wordpress伪静态链接
tags:
  - Apache
  - WordPress
id: '919'
categories:
  - - 学习笔记
  - - 文章
date: 2022-04-08 18:02:48
---

### 设置Wordpress伪静态链接

#### Apache2.4

> 本文是不在使用宝塔的情况下自行设定的, 有安装宝塔的请忽视
> 
> 环境：Ubuntu20.04 Apache2.4 Wordpress5.9.2

##### 重写规则

*   开启重写规则
    
    ```c
    sudo a2enmod rewrite
    ```
    
*   设置根目录重定向
   <!-- more --> 
    ```
    sudo vim /etc/apache2/apache2.conf
    ```
    
    找到 `<Directory /var/www/>`
    
    ![image-20220408175414794](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220408175414794.png)
    
    将`AllowOverride` 后的 `None` 修改为 `ALL` 。
    
*   进入Wordpress后台选择自己喜欢的伪静态，并点击保存，自动生成 `.hatccess` 规则并复制，如下：
    
    ![image-20220408174428359](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220408174428359.png)
    
    ```C
    
    RewriteEngine On
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteRule ^^unsubscribe-comment-mailnotice/?(.*)$ //wp-con>
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    ```
    
*   在刚刚的 `/var/www/` 目录下(即网站的根目录) 创建 `.hatccess` 文件
    
    粘贴刚刚复制的规则代码并保存。
    
*   重启服务器即可
    
    ```
    sudo systemctl restart apache2
    ```
    

##### 效果图

![image-20220408180040599](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220408180040599.png)

#### Nginx1.19

> 本文是不在使用宝塔的情况下自行设定的, 有安装宝塔的请忽视
> 
> 环境：Raspi OS 64bit(Debian11) Wordpress5.9.2

打开配置文件 `sudo vim /etc/nginx/sites-available/default` 如果wordpress安装在网站根目录，在server中添加

```
location / {  
                if (!-e $request_filename) {  
                rewrite (.*) /index.php;  
                }  
        }  
```

如果wordpress安装在网站二级目录，在server中添加:

```
location /二级目录/ {  
                if (!-e $request_filename) {  
                rewrite (.*) /cn/index.php;  
                }  
        }
```

重启 `sudo /etc/init.d/ngnix restart` 在wordpress后台设置伪静态即可