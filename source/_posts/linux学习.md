---
title: Linux学习笔记
tags:
  - Linux
id: '964'
categories:
  - - 学习笔记
date: 2022-04-17 10:27:53
---

整理了一下学习Linux命令的笔记，特发此文，后续继续更新。

在初学Linux时推荐两种方法：

1.  去相关社区、在线查询网站学习交流，我推荐两个：
    
    [Linux工具快速教程 — Linux Tools Quick Tutorial (linuxtools-rst.readthedocs.io)](https://linuxtools-rst.readthedocs.io/zh_CN/latest/)
    
    [Linux命令大全(手册) – 真正好用的Linux命令在线查询网站 (linuxcool.com)](https://www.linuxcool.com/)
    
2.  使用`man 命令` 查看帮助文档
    

### Shell
<!-- more -->
##### echo

输出字符串或提取Shell变量的值

`$变量` ：提取变量的值

`echo $PATH` ：提取PATH环境变量并输出

![image-20220407160502862](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220407160502862.png)

`$?`：提取最近一次Shell命令的返回值（退出状态）

0表示没有错误，其它表示有错误

![image-20220411172602554](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220411172602554.png)

##### 一般变量

Linux下变量无需声明，一般做字符串处理，数值计算时转化为数字

*   创建或修改变量：`变量名=变量值` (中间不能有空格)
    
*   显式变量值：`echo $变量名`
    
*   删除变量：`unset 变量名`
    
*   导出变量名：`export 变量名`
    

![image-20220409195301371](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220409195301371.png)

##### 特殊的Shell变量

`$!`:后台运行的最后一个进程的ID号

`$@`:与$\*相同，但是使用时加引号，并在引号中返回每个参数，所有参数分解为包含若干个字符串的数组

`$#`:传递给脚本或者函数的参数个数

`$$`:执行本脚本程序的PID值

`$*`:所有参数组合成的一个字符串

`$?`:上一条语句的返回值

`$0`:脚本程序自身的名称(命令行名称)

`$1`、`$2`、`$3`:传给脚本或者函数的第一、二、三个参数

编写一个shell脚本：

```sh
#!/bin/bash
if [ $# -gt 1 ];
then
        echo "\$0程序名称:$0,\$1第一个参数是:$1"
        echo "\$2第二个参数是:$2,\$*所有参数组合的字符串:$*"
else
        echo "you need input beyond 2 pram"
fi
echo "\$$程序运行的PID值:$$"
```

分别用`./specialshell.sh`和`source specialshell.sh`执行脚本，结果：

![image-20220411181826008](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220411181826008.png)

##### shell脚本

当前目录创建一个test.sh, 加入:

```shell
#!/bin/bash
for x in apple banna cake fruits
do
        echo "I love eat $x"
        sleep 1
done
```

执行 `source test.sh`

![image-20220411105352232](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220411105352232.png)

##### for语句

```sh
for var in list
do
     statements
done
```

##### until语句

```sh
until [expression]
do
    statements
done
```

##### if语句

```sh
if [expression];
then
    statements
elif [expression];
then
    statements
else
    statements
fi
```

##### while语句

```sh
while expression
do
    statements
done
```

##### 函数

```sh
function 函数名()
{
    statements
}
```

##### test命令

用来做字符串比较、数值比较、文件测试、逻辑操作符。

字符串比较

符号

含义

\=

比较两个字符串是否相等。如：test "1" = "2"

!=

比较两个字符串是否不等

\-n

检查字符串长度是否大于0。如：test -n ""

\-z

检查字符串长度是否等于0

数值比较

符号

含义

\-eq

比较两个数值是否相等

\-ge

比较前者是否大于等于后者

\-le

比较后者是否大于等于前者

\-ne

比较两个数值是否不等

\-gt

比较前者是否大于后者

\-lt

比较前者是否小于后者

文件测试

符号

含义

\-d

检查是否是一个目录。如：test -d .inputrc

\-f

检查是否是一个文件

\-e

检查文件名或者目录名是否存在

\-r

检查对此文件是否有"读"权限

\-s

检查文件长度是否大于0

\-w

检查对此文件是否有"写"权限

\-x

检查对此文件是否有"执行"权限

逻辑操作

符号

含义

!

逻辑非（NOT）。如：test ! 1 -lt 2

\-a

逻辑与（AND）。如：test 1 -lt 2 -a 2 -gt 3

\-o

逻辑或（OR）。如：test 1 -lt 2 -o 2 -lt 3

### **GCC**

gcc是多种语言、自由、跨平台的编译器

##### 编译流程

GCC将源代码便以为可执行程序的流程

1.  预处理(Preproccessing)
    
2.  编译(Compilation)
    
3.  汇编(Assemble)
    
4.  链接(Linking)
    

常用参数

1.  \-version 查看版本
2.  \-v 输出编译的详细信息
3.  \-std 指定标准
4.  \-o 指定输出文件的名称
5.  \-Wall 输出所有警告信息
6.  \-c 直将源文件编译为object文件(.o)，而不进行链接，之后可用`gcc -o 可执行文件名称 out1.o out2.o out3.o`链接为可执行文件
7.  \-shared 编译为共享库(\*.dll，.so)
8.  \-S 编译为汇编代码

### **命令**

##### useradd \[参数\] 用户名

新建一个用户

参数

作用

\-D

改变新建用户的预设值

\-c

添加备注文字

\-d

新用户每次登录时所使用的家目录

\-e

用户终止日期，格式为YYYY-MM-DD

\-f

用户过期几日后永久停权。当值为0时用户立即被停权，而值为-1时则关闭此功能，预设值为-1

\-g

指定用户对应的用户组

\-G

定义此用户为多个不同组的成员

\-m

用户目录不存在时自动创建

\-M

不建立用户家目录，优先于/etc/login.defs文件设定

\-n

取消建立以用户名称为名的群组

\-r

建立系统账号

\-u

指定用户id

仅仅只是用`useradd 用户名`这个命令不能创建一个可以登录使用的用户，/home目录下没有对应的用户，也创建不了密码。

需要用`useradd -m 用户名`创建一个可登录的，可以创建密码的用户。创建后可以在etc目录下的passwd添加这个新用户的相关信息。

添加新用户www：

```
[root@www ~]# useradd www
```

不创建家目录，并且禁止登陆：

```
[root@www ~]# useradd -M -s /sbin/nologin www
```

添加新用户www，指定UID为666，指定shell类型为/bin/bash，指定归属用户组为root，cool成员：

```
[root@www ~]# useradd -u 666 -s /bin/bash -G root,cool www
```

添加新用户www，设置家目录为/tmp/www，用户过期时间为2030/01/01.过期后两天停权：

```
[root@www ~]# useradd -e "2030/01/01" -f 2 -d /tmp/www www
```

此外，新增的用户不具有`sudo`权限，需要手动在`/etc/sudoer`增加新用户权限

选项

说明

user ALL=(ALL) ALL

允许用户user执行sudo命令(需要输入密码).

%user ALL=(ALL) ALL

允许用户组user里面的用户执行sudo命令(需要输入密码).

user ALL=(ALL) NOPASSWD: ALL

允许用户user执行sudo命令,并且在执行的时候不输入密码.

%user ALL=(ALL) NOPASSWD: ALL

ALL=(ALL)允许用户组user里面的用户执行sudo命令,并且在执行的时候不输入密码.

![image-20220416233152507](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220416233152507.png)

更多信息查看文档 `man useradd`

##### usermod \[参数\] 用户名

改变用户的信息。其中参数选项的作用与 `useradd` 类似。

##### who

产看当前shell用户

![image-20220417103032111](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220417103032111.png)

##### id

查看当前用户的身份以及权限

![image-20220409194409671](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220409194409671.png)

##### make

Makefile 支持多线程并发操作make 命令只会编译我们修改过的文件，没有修改的文件不用重新编译

Makefile 描述的是文件编译的相关规则，它的规则主要是两个部分组成，分别是依赖的关系和执行的命令

##### umask \[-S\] (权限掩码)

新建初始化文件的权限掩码

```shell
umask #获取当前权限掩码
umask -S #获取当前权限的可读权限信息
umask 0000 #修改新建初始化目录文件的权限
```

![image-20220414102213730](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220414102213730.png)

![image-20220414102305203](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220414102305203.png)

666& umask

新建初始化目录的权限

777& umask

##### ls \[参数\] (正则表达式)

文件的分类

普通文件 `-` , 目录文件 `d` , （软）链接文件 `l` , 字符设备文件 `c` , 块设备文件 `b` , 管道文件 `p`

目录不是目录，是一种目录文件

目录

*   r: 可以ls目录中的内容
*   w: 可以删除增加目录文件
*   x: 可以cd进入这个目录

文件

*   r：read 可读
*   w：write 可写
*   x：增删
*   r：可运行

`ls -ld /etc` ： 查看目录拥有权限

![image-20220407151622771](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220407151622771.png)

##### chmod \[选项\] \[文件或目录\]

赋予文件或目录权限

`chmod u=wx etc` ：赋予etc目录 `w:增删` 和 `x:进入` 的权限

`chmod u-r etc` ：删去etc目录 `r：可ls` 的权限

其中还可以用三位十进制数表示不同权限

> r :含义为 “可读”，用数字 4 表示 w:含义为 “可写”用数字 2 表示 x：含义为“可执行”用数字 1 表示 -：含义为“无权限”用数字0 表示

所有者

群组

其他

三位代表权限的数字

rwx

rwx

rwx

实际结果

421

421

421

777

421

401

401

705

```sh
chmod 777 a.out 
#777给auth.log文件赋予任何可读，可写，可执行权限
chmod 755 a.out
#755代表用户对该文件拥有读，写，执行的权限，同组和其它用户有读和执行权限，没有写权限
```

##### which \[命令\]

查看命令的安装路径

![image-20220407160526521](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220407160526521.png)

##### mout \[参数\]

挂载文件系统

> 挂载：是指由操作系统使一个 `存储设备` (如硬盘、CD-ROM或共享资源)上的计算机文件或目录可供用户通过计算机的 `文件系统` 访问的一个过程。
> 
> 引自——[挂载\_百度百科 (baidu.com)](https://baike.baidu.com/item/挂载)

当访问挂载点的时候，系统就知道要用哪些数据组织形式访问哪些类型的文件系统(或者说是哪些物理设备)。

> 作用：将一个具体存储设备上的具体文件系统和操作系统中对应的文件系统驱动(模块)关联起来，并将这个具体文件系统中的文件和目录关系挂载到全局目录树上，形成一个“激活运行状态”的文件系统。
> 
> 引自——[https://www.zhihu.com/answer/2437952746](https://www.zhihu.com/answer/2437952746)

将/dev/sdb1分区挂载到/wg目录上的命令：`mount /dev/sdb1 /wg`

umount实现文件系统的卸载

卸载/wg上的文件系统的命令：`umount /wg`

##### export \[参数\]

将shell变量/函数输出为环境变量

`-p` ：列出所有shell赋予程序的环境变量

`-n` ：删除指定变量

定义环境变量：`# export MYENV`

赋值：`# export MYENV=1111`

##### fdisk \[选项\] \[设备\]

操纵磁盘分区表

`-l`：列出所有分区表

##### df \[参数\] \[指定文件\]

显示磁盘空间使用情况

![image-20220407141532576](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220407141532576.png)

`-a` ：显示所有系统文件

`-h` ：以易于human的阅读方式显示

`-l` ：只显示本地文件

##### pwd

打印当前工作目录(print working directory)

##### source \[指定文件\]

source命令通常用于执行刚修改的 `初始化` 文件，使之立即生效，不用注销重新登陆？

##### env

查看Shell环境变量

##### stat

##### netstat

`netstat -ant grep 3306`

![image-20220407233817307](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220407233817307.png)

##### fflush

##### kill \[参数\]

### **文件**

##### /etc/passwd

系统用户配置文件，存储了系统中所有用户的基本信息。

权限：所有用户可读

```c
sudo vim /etc/passwd
```

![image-20220409193721743](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220409193721743.png)

名称

说明

用户名

已创建的用户名

加密口令

x代表加密密码保存在`/etc/shadow`文件中

用户ID

代表用户的ID号，每个用户都有一个唯一的ID

组ID

代表群组的ID好，每个群组都有一个唯一的ID

账号说明

描述用户的信息

主目录

代表用户所在的家目录

登录Shell

代表用户使用Shell的类型

![image-20220415181954137](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220415181954137.png)

##### /etc/profile

建立全系统通用的初始环境变量，每次用户登录时 `第一个` 被执行

##### /etc/fstab

系统启动时自动挂载。当系统启动时，系统会自动地从这个文件读取信息，并且会自动将此文件中指定的文件系统挂载到指定内目录。

### 额外

##### 禁止root用户

禁止root用户登录，并新建一个自己的用户

1.  新建wyz用户（具体查看useradd）
    
2.  赋予wyz 用户 sudo权限
    
3.  禁止root登录
    
    `$ sudo vim /etc/ssh/sshd_config`
    
    将`PermitRootLogin yes`更改为 `no`
    
    ![image-20220417163830390](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220417163830390.png)
    

##### 进程

可执行(二进制)程序被系统加载到内存空间运行时，就是 `进程`。

每个进程都有唯一的标识号—— `PID`，进程可以产生新的进程，构成父子关系并形成进程树(pstree)。

![image-20220417164549862](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220417164549862.png)

进程都有一个 `用户标识`——对应运行此程序的用户ID。如果可执行文件没有设置`suid`和`sgid`，则进程的 `有效用户标识` 为此用户，否则为文件的所有者。

每一个进程属于且仅属于一个 `进程组`，向进程组发送信号，则组内所有进程都能收到。

每个进程都属于一个唯一的 `会话` 。用户登录后产生一个`会话`，会话包含若干进程组。会话的id是首进程组的id。一个会话中只有一个进程组是前台进程组，和控制终端交互，获取输入，接收信号。

控制终端关闭时，进程会收到 `SIGHUP` 信号。

除了PID为 0 的进程外，所有进程都有父进程。

进程优先级 = 优先级别(PR) + 谦让值(NI)。优先级别从父进程继承而来，不可更改，可以用 `nice` 将进程默认的谦让值从 0 改大(或改小)。

##### 环境变量

全局的环境变量：存放在 `/etc/profile`![image-20220417174359704](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220417174359704.png)

特定用户的环境变量：存放在home目录的 `.profile`或 `.bashhrc`中![image-20220417173635469](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220417173635469.png)

临时的环境变量：`export`定义，特定于此会话