---
title: Python脚本自动更新PIP源
tags:
  - Python
id: '980'
categories:
  - - 学习笔记
date: 2022-04-18 16:47:02
---

默认的pip源都在国外服务器上，下载速度慢，为了提高下载包的速度，每次都需要加上国内镜像源镜像，或者自己加上个pip配置文件，Linux为`pip.conf`，Windows为`pip.ini`。因此可以使用一个python脚本自动帮我们配置好。 其中配置文件的格式都为：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
```

源码 pipupdate.py:
<!-- more -->
```python
#!/usr/bin/python
# coding: utf-8

import platform
import os

os_type = platform.system()
if "Linux" == os_type:
    fileDirPath = "%s/.pip" % os.path.expanduser('~')
    filePath = "%s/pip.conf" % fileDirPath
    if not os.path.isdir(fileDirPath):
        os.mkdir(fileDirPath)
    fo = open(filePath, "w")
    fo.write(
        "[global]\nindex-url=https://pypi.tuna.tsinghua.edu.cn/simple/\n[install]\ntrusted-host=pypi.tuna.tsinghua.edu.cn\n")
    fo.close()
    print("Configuration is complete")
elif "Windows" == os_type:
    fileDirPath = "%s\\pip" % os.path.expanduser('~')
    filePath = "%s\\pip.ini" % fileDirPath
    if not os.path.isdir(fileDirPath):
        os.mkdir(fileDirPath)
    fo = open(filePath, "w")
    fo.write(
        "[global]\nindex-url=https://pypi.tuna.tsinghua.edu.cn/simple/\n[install]\ntrusted-host=pypi.tuna.tsinghua.edu.cn\n")
    fo.close()
    print("Configuration is complete")
else:
    exit("Your platform is unknow!")
```

上述例子以清华大学镜像源为例，其中还可以将`index-url`和`trusted-host`换成别的镜像源 国内几个镜像源：

> 阿里云 [http://mirrors.aliyun.com/pypi/simple/](http://mirrors.aliyun.com/pypi/simple/) 中国科技大学 [https://pypi.mirrors.ustc.edu.cn/simple/](https://pypi.mirrors.ustc.edu.cn/simple/) 豆瓣(douban) [http://pypi.douban.com/simple/](http://pypi.douban.com/simple/) 清华大学 [https://pypi.tuna.tsinghua.edu.cn/simple/](https://pypi.tuna.tsinghua.edu.cn/simple/) 中国科学技术大学 [http://pypi.mirrors.ustc.edu.cn/simple/](http://pypi.mirrors.ustc.edu.cn/simple/)

直接运行源代码即可完成配置 ![image-20220418164541419](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220418164541419.png)

此外，如果pip版本>=10.0.0，可以使用如下命令进行设置：

```
pip config set global.trusted-host  mirrors.aliyun.com
pip config set global.index-url http://mirrors.aliyun.com/pypi/simple/
```

###### References

[https://www.cnblogs.com/sunnydou/p/5801760.html](https://www.cnblogs.com/sunnydou/p/5801760.html)