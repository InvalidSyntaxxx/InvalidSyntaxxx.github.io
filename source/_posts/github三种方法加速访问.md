---
title: GitHub三种方法加速访问
tags:
  - 分享
id: '1055'
categories:
  - - 学习笔记
date: 2022-05-06 00:57:34
---

## 一、镜像网站(不推荐)

这里提供两个最常用的镜像地址：

*   [https://github.com.cnpmjs.org](https://github.com.cnpmjs.org)
*   [https://hub.fastgit.org](https://hub.fastgit.org/)
*   [GitHub: Where the world builds software · GitHub (fastgit.xyz)](https://hub.fastgit.xyz/)

有时候并不生效，依然被q，而且镜像网站随时可能跑路

## 二、修改hosts文件(安全、快速、不稳定)

关于`hosts`文件：`hosts` 文件本来是用来**提高解析效率**。 在进行 DNS 请求以前，系统会先检查自己的 `hosts` 文件中是否有这个地址映射关系，如果有则调用这个 IP 地址映射，如果没有再向已知的 DNS 服务器提出域名解析。

也就是 `hosts`\> `DNS`
<!-- more -->
1.  找到hosts文件
    
    `hosts`文件在不同操作系统中的存放路径：
    
    **Windows系统：**
    
    > C:\\Windows\\System32\\drivers\\etc
    
    ![image-20220505235444028](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220505235444028.png)
    
    **Mac OS、Linux及其它类Unix系统：**
    
    > /etc/
    
    **Android系统：**
    
    > /system/etc/
    
2.  添加域名解析
    
    在`hosts`文件末尾添加上以下内容：
    

```python
   # GitHub Start
140.82.113.25                 alive.github.com
140.82.114.26                 live.github.com
185.199.108.154               github.githubassets.com
140.82.114.21                 central.github.com
185.199.108.133               desktop.githubusercontent.com
185.199.108.153               assets-cdn.github.com
185.199.108.133               camo.githubusercontent.com
185.199.108.133               github.map.fastly.net
199.232.69.194                github.global.ssl.fastly.net
140.82.113.3                  gist.github.com
185.199.108.153               github.io
140.82.114.3                  github.com
192.0.66.2                    github.blog
140.82.114.6                  api.github.com
185.199.108.133               raw.githubusercontent.com
185.199.108.133               user-images.githubusercontent.com
185.199.108.133               favicons.githubusercontent.com
185.199.108.133               avatars5.githubusercontent.com
185.199.108.133               avatars4.githubusercontent.com
185.199.108.133               avatars3.githubusercontent.com
185.199.108.133               avatars2.githubusercontent.com
185.199.108.133               avatars1.githubusercontent.com
185.199.108.133               avatars0.githubusercontent.com
185.199.108.133               avatars.githubusercontent.com
140.82.113.9                  codeload.github.com
52.216.137.116                github-cloud.s3.amazonaws.com
52.217.203.169                github-com.s3.amazonaws.com
52.217.105.68                 github-production-release-asset-2e65be.s3.amazonaws.com
52.217.203.169                github-production-user-asset-6210df.s3.amazonaws.com
52.216.241.132                github-production-repository-file-5c1aeb.s3.amazonaws.com
185.199.108.153               githubstatus.com
64.71.144.211                 github.community
40.68.78.177                  github.dev
140.82.113.21                 collector.github.com
13.107.42.16                   pipelines.actions.githubusercontent.com
185.199.108.133               media.githubusercontent.com
185.199.108.133               cloud.githubusercontent.com
185.199.108.133               objects.githubusercontent.com
 # GitHub End
```

**注意**：这些ip可能会发生改变，具体可自行百度搜索相关ip信息，该记录我使用了半年之久都有效。

3.  刷新DNS缓存使之生效：
    
    Windows系统在cmd命令行中执行命令：
    
    ```sh
    ipconfig/flushdns
    ```
    
    ![image-20220505235658796](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220505235658796.png)
    
    Linux等系统一般修改后都即刻生效。
    
4.  Github查看效果
    
5.  总结
    
    这种方法其实也不稳定，大部分时候是能正常快速访问，我也用了一年左右，但是还是会有时不时被墙。个人更喜欢这个~
    

## 三、代理(稳定高速、部分收费)

在这里我只是介绍一个使用代理服务快速访问github的方法，由该方法导致任何的后果概不负责。

有很多种方式加速访问github，听过的就有stream++、epic++、XXXX等，自行百度吧。在这里我介绍使用github上的一个项目[Fastgithub](https://github.com/dotnetcore/FastGithub)来加速访问github，特点：免费高效、开源。

1.  其实github上面就有很详细的教程，我只是复制一遍，方便目前还访问不了的人，以下是原文档，如有侵权请联系我删除：
    
    > #### 2.1 windows-x64桌面
    > 
    > *   双击运行FastGithub.UI.exe
    > 
    > #### 2.2 windows-x64服务
    > 
    > *   `fastgithub.exe start` // 以windows服务安装并启动
    > *   `fastgithub.exe stop` // 以windows服务卸载并删除
    > 
    > #### 2.3 linux-x64终端
    > 
    > *   `sudo ./fastgithub`
    > *   设置系统自动代理为`http://127.0.0.1:38457`，或手动代理http/https为`127.0.0.1:38457`
    > 
    > #### 2.4 linux-x64服务
    > 
    > *   `sudo ./fastgithub start` // 以systemd服务安装并启动
    > *   `sudo ./fastgithub stop` // 以systemd服务卸载并删除
    > *   设置系统自动代理为`http://127.0.0.1:38457`，或手动代理http/https为`127.0.0.1:38457`
    > 
    > #### 2.5 macOS-x64
    > 
    > *   双击运行fastgithub
    > *   安装cacert/fastgithub.cer并设置信任
    > *   设置系统自动代理为`http://127.0.0.1:38457`，或手动代理http/https为`127.0.0.1:38457`
    > *   [具体配置详情](https://github.com/dotnetcore/FastGithub/blob/master/MacOSXConfig.md)
    > 
    > #### 2.6 docker-compose一键部署
    > 
    > *   准备好docker 18.09, docker-compose.
    > *   在源码目录下，有一个docker-compose.yaml 文件，专用于在实际项目中，临时使用github.com源码，而做的demo配置。
    > *   根据自己的需要更新docker-compose.yaml中的sample和build镜像即可完成拉github.com源码加速，并基于源码做后续的操作。
    > 
    > ### 3 软件功能
    > 
    > *   提供域名的纯净IP解析；
    > *   提供IP测速并选择最快的IP；
    > *   提供域名的tls连接自定义配置；
    > *   google的CDN资源替换，解决大量国外网站无法加载js和css的问题；
    > 
    > ### 4 证书验证
    > 
    > #### 4.1 git
    > 
    > git操作提示`SSL certificate problem` 需要关闭git的证书验证：`git config --global http.sslverify false`
    > 
    > #### 4.2 firefox
    > 
    > firefox提示`连接有潜在的安全问题` 设置->隐私与安全->证书->查看证书->证书颁发机构，导入cacert/fastgithub.cer，勾选“信任由此证书颁发机构来标识网站”
    > 
    > ### 5 安全性说明
    > 
    > FastGithub为每台不同的主机生成自颁发CA证书，保存在cacert文件夹下。客户端设备需要安装和无条件信任自颁发的CA证书，请不要将证书私钥泄露给他人，以免造成损失。
    > 
    > ### 6 合法性说明
    > 
    > 《国际联网暂行规定》第六条规定：“计算机信息网络直接进行国际联网，必须使用邮电部国家公用电信网提供的国际出入口信道。任何单位和个人不得自行建立或者使用其他信道进行国际联网。” FastGithub本地代理使用的都是“公用电信网提供的国际出入口信道”，从国外Github服务器到国内用户电脑上FastGithub程序的流量，使用的是正常流量通道，其间未对流量进行任何额外加密（仅有网页原有的TLS加密，区别于VPN的流量加密），而FastGithub获取到网页数据之后发生的整个代理过程完全在国内，不再适用国际互联网相关之规定。
    
2.  下载文件
    
    这是总的下载链接：[https://github.com/dotnetcore/FastGithub/releases/latest](https://github.com/dotnetcore/FastGithub/releases/latest)
    
    截止到2022年5月6号最新的版本：
    
    [fastgithub\_linux-arm64.zip](https://github.com/dotnetcore/FastGithub/releases/download/2.1.4/fastgithub_linux-arm64.zip)
    
    [fastgithub\_linux-x64.zip](https://github.com/dotnetcore/FastGithub/releases/download/2.1.4/fastgithub_linux-x64.zip)
    
    [fastgithub\_osx-arm64.zip](https://github.com/dotnetcore/FastGithub/releases/download/2.1.4/fastgithub_osx-arm64.zip)
    
    [fastgithub\_osx-x64.zip](https://github.com/dotnetcore/FastGithub/releases/download/2.1.4/fastgithub_osx-x64.zip)
    
    [fastgithub\_win-x64.zip](https://github.com/dotnetcore/FastGithub/releases/download/2.1.4/fastgithub_win-x64.zip)
    
    对于下载慢的可以试着用[github文件下载加速网站](https://www.baidu.com/s?wd=github%E6%96%87%E4%BB%B6%E4%B8%8B%E8%BD%BD%E5%8A%A0%E9%80%9F%E7%BD%91%E7%AB%99)进行下载，这里推荐几个当前可用的加速下载网站
    
    [https://gh.api.99988866.xyz](https://gh.api.99988866.xyz)
    
    [https://ghproxy.com/](https://ghproxy.com/)
    
    [https://toolwa.com/github/](https://toolwa.com/github/)
    
3.  运行解压后的文件，以我的Windows10 64位系统为例
    
    选择任意一个.exe运行程序运行即可
    
    ![image-20220506003759569](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220506003759569.png)我运行`fastgithub.exe`的截图![image-20220506003935842](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220506003935842.png)
    

接下来进入github的大门吧

## ![image-20220506004312524](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220506004312524.png)

#### 题外话：

github被 ‘半墙’是很让人恼火的，有时候PC访问不了，但是移动端又稳定得一批（没改hosts、没开代理）不管怎样，上面有很多很好地项目源码，我们学习离不开它。不过最近github上面开始出现了一些zz因素、传教等等，我们有义务保持理性的态度去面对，也不知道哪一天github也参与zz中来，搞的我们的项目代码不能正常使用。（本人开始试试自己搭建属于自己的git服务了，比如gitlab好像是不错的选择，后期更新看看）