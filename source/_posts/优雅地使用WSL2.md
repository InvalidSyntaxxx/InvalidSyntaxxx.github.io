---
title: 优雅地使用WSL2
tags:
  - Linux
  - Windows
id: '1035'
categories:
  - - 学习笔记
  - - 工具
date: 2022-04-24 12:24:26
# cover: https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424002629616.png
thumbnail: https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424002629616.png
toc: true
---

记录一次安装WSL 2的过程...

#### 什么是WSL2

WSL全称为Windows Subsystem for Linux，官网译为：适用于 Linux 的 Windows 子系统 (WSL)。

官方文档直达：[适用于 Linux 的 Windows 子系统文档 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/wsl/)
<!-- more -->
[WSL1和WSL2的比较](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions):

| 功能                                           | WSL 1 | WSL 2 |
| :--------------------------------------------- | :---- | :---- |
| Windows 和 Linux 之间的集成                    | ✅     | ✅     |
| 启动时间短                                     | ✅     | ✅     |
| 与传统虚拟机相比，占用的资源量少               | ✅     | ✅     |
| 可以与当前版本的 VMware 和 VirtualBox 一起运行 | ✅     | ✅     |
| 托管 VM                                        | ❌     | ✅     |
| 完整的 Linux 内核                              | ❌     | ✅     |
| 完全的系统调用兼容性                           | ❌     | ✅     |
| 跨 OS 文件系统的性能                           | ✅     | ❌     |

#### 为什么要WSL2

官方解释：可让开发人员直接在 Windows 上按原样运行 GNU/Linux 环境（包括大多数命令行工具、实用工具和应用程序），且不会产生传统虚拟机或双启动设置开销。

我的观点：日常生活中程序的开发离不开Linux，而Windows的GUI界面又是我们常用的（微信、Office等）。我们可以有很多种方式使用Linux，如：

| 方案         | 优点                                                 | 缺点                                                         |
| ------------ | ---------------------------------------------------- | ------------------------------------------------------------ |
| 单主机双系统 | 能实实在的运行不同、完整的操作系统                   | 切换系统都需要重启，麻烦                                     |
| 双主机双系统 | 物理隔离方式，真正实现双系统                         | 真的有人那么有钱吗？开发程序用两台电脑？如果有，请问土豪缺朋友吗😁 |
| 远程服务器   | 和单主机双系统一样                                   | 性能、带宽、流量有局限                                       |
| 虚拟机VMware | 和单主机双系统一样                                   | 资源消耗大、启动慢、运行效率低。我用过之后觉得有的时候卡死也不知道怎么弄。。 |
| WSL！！！    | 几乎能运行完整的操作系统，资源消耗小、启动快、切换快 | 有些软件可能不支持...（后续有什么毛病再更新）                |

重点：Windows与Linux子系统将共用同一文件系统!!!!  我们可以在WSL中使用三剑客命令查询分析windows文档、日志、使用shell命令或者bash脚本运行存储在windows中的linux程序、甚至在WSL中创建docker容器，在windows下使用docker desktop进行可视化管理。

总结：<u>**WSL2让我们既拥有Windows的操作界面又拥有Linux的命令行工具。**</u>

#### 启用“虚拟机平台”

WSL 2 需要启用 Windows 10 的 “虚拟机平台” 特性。它独立于 Hyper-V，并提供了一些在 Linux 的 Windows 子系统新版本中可用的更有趣的平台集成。

要在 **Windows 10（2004）**上启用虚拟机平台，请以管理员身份打开 PowerShell 或  cmd 并运行：

```shell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

要在 **Windows 10（1903，1909）**上启用虚拟机平台，请以管理员身份打开 PowerShell或  cmd 并运行：

```shell
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -NoRestart
```

为了确保所有相关部件都整齐到位，您应该在**此时重启系统**，否则可能会发现事情没按预期进行。

![image-20220424002629616](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424002629616.png)

#### 安装WSL2

本次安装环境

>处理器	  Intel(R) Core(TM) i5-10210U CPU @ 1.60GHz   2.11 GHz
>机带 RAM	   8.00 GB (7.79 GB 可用)
>系统类型	   64 位操作系统, 基于 x64 的处理器
>操作系统	   Windows 10 家庭中文版

注意：本次安装之前没安装过WSL和Ubuntu，只运行过VMware虚拟机。

##### 检查是否可以安装

[您的电脑需要以下配置](https://www.cnblogs.com/ittranslator/p/14128570.html)：

- Windows 10 2020年5月(2004) 版, Windows 10 2019年5月(1903) 版，或者 Windows 10 2019年11月(1909) 版
- 一台支持 Hyper-V 虚拟化的计算机

查看是否支持Hyper-V的方法：

- 打开cmd，输入

  ```shell
  systeminfo
  ```

- 查看Hyper-V信息

  比如我的电脑就可以支持

  ![image-20220423221854978](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220423221854978.png)

##### 安装WSL

用管理员身份运行PowerShell

```sh
wsl --install
```

--install 命令执行以下操作：

- 启用可选的 WSL 和虚拟机平台组件
- 下载并安装最新 Linux 内核
- 将 WSL 2 设置为默认值
- 下载并安装 Ubuntu Linux 发行版（**可能需要重新启动**）

[注意](https://docs.microsoft.com/zh-cn/windows/wsl/install)： 上述命令仅在完全未安装 WSL 时才有效，如果运行 `wsl --install` 并查看 WSL 帮助文本，请尝试运行 `wsl --list --online` 以查看可用发行版列表并运行 `wsl --install -d <DistroName>` 以安装发行版。

![image-20220423221123835](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220423221123835.png)

等待一会会，去打局游戏再回来....

顺便查看了一下可以支持的linux系统，大便、Kali、OpenSUSE、乌班图都有，默认安装Ubuntu。

```shell
wsl --list --online
```

![image-20220423223723552](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220423223723552.png)

----

安装了好一会儿了。。。。发现还是在85.7%，等不下去了`CTRL+C`了。

重新安装，这次安装指定的系统

```shell
wsl --install -d Ubuntu-20.04
```

![image-20220423224013320](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220423224013320.png)

重启，然后成功了!

---

##### 配置Linux

接下来打开已安装的Ubuntu，这时候会提示你配置用户和密码

配置完毕！即可享用

测试一下，用命令 `cd / && ls -la` 查看所有文件，如下（是不是很熟悉）

![image-20220424000945772](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424000945772.png)

更新一下镜像源：

```shell
sudo vim /etc/apt/sources.list
```

将官方的源都注释掉，换成下面两个之一即可（我的是Ubuntu20.04，别的版本或者源可以自行网上搜）

- 阿里源

```shell
deb https://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

- 清华源

```shell
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse 
```

Debian系统分好几种，`wheezy`、`jessie`、`stretch`、`buster`，它们分别对应：

| Debian版本 | 对应名称 |
| :--------: | :------: |
|  Debian7   |  wheezy  |
|  Debian8   |  jessie  |
|  Debian9   | stretch  |
|  Debian10  |  buster  |
|  Debian11  | bullseye |

Debian 11（Bullseye）国内镜像源：

- 阿里

```shell
deb https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb-src https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
```

- 清华

```shell
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
```

##### 遇到问题：

Certificate verification failed: The certificate is NOT trusted——更新Ubuntu20.04、Debian11的过程中遇到的证书验证失败问题。

解决办法：

1. 更改源文件，将所有的https改成http

  ```sh
sudo nano /etc/apt/sources.list
  ```

2. 重新更新源

  ```shell
sudo apt update
  ```

3. 安装/更新证书ca-certificates

  ```shell
sudo apt install --reinstall ca-certificates
  ```

4. 参照步骤一将镜像源文件改回https

5. 再次更新源

  ```shell
sudo apt update && sudo apt upgrade
  ```

6. 大功告成

#### 安装Windows Terminal

Windows Terminal能帮助我们管理命令行工具、PowerShell和WSL等Shell用户的工具，能为我们提供最佳的 WSL 体验。

下载方式

- https://www.microsoft.com/store/productId/9N0DX20HK701
- MicroSoft Store （微软商店）找关键字 `Windows Terminal`

![image-20220424115803757](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424115803757.png)

下载安装即可。功能确实很多哈哈哈，效果：

![WindowsTerminal_HCukprnOs0](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/WindowsTerminal_HCukprnOs0.gif)

---

遇到的问题：过程中下载失败了好多次我不断点击重新下载才成功。

#### 查看Linux版本信息

1. `cat  /etc/os-release`

   ![image-20220424132217753](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424132217753.png) 

2. `cat  /proc/version`

   ![image-20220424132605991](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424132605991.png)

3. `uname -a`

4. ![image-20220424133412380](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424133412380.png)

5. `lsb_release -a`

   ![image-20220424133534249](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220424133534249.png)

6. `neofetch`

   ![image-20220502182008974](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220502182008974.png)

#### 总结

刚安装了还不知道怎么样，看网上的说法褒贬不一，我也在不断尝试，后续再更，说说感受。

参考：

[Winux之路-WSL 2的使用及填坑 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/224753478)

[适用于 Linux 的 Windows 子系统文档 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/wsl/)


