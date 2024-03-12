---
title: 树莓派4B点亮LED(Python实现)
tags:
  - Linux
  - Python
  - 树莓派
id: '644'
categories:
  - - 学习笔记
  - - 文章
date: 2022-03-14 23:43:33
---

#### 1、树莓派GPIO引脚图

![rpi-pins-40-0](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/rpi-pins-40-0.png)

\[admonition title="注意" color="red"\] 先简单了解一下引脚，我们对树莓派引脚的操作是有可能损坏我们的树莓派的。有必要提前了解。\[/admonition\]

功能名：

*   绿色背景：GPIO是标准引脚，可以用来打开和关闭设备。例如，一个LED。
*   浅蓝色背景：I2C(Inter-Integrated Circuit)引脚连接并与支持该协议(I2C协议)的硬件模块对话。这个协议通常会占用两个引脚。
*   紫色背景：SPI（串行外设接口总线）引脚可用于连接和对话SPI设备。和I2C差不多，但使用了不同的协议。
*   深蓝色背景：UART(Universal asynchronous receiver/transmitter，通用异步接收/发送器)是用于与其他设备通信的串行引脚。
*   黑色：GND是用来接地的引脚。使用哪个引脚并不重要，因为它们都连接在同一条线上。
<!-- more -->
#### 2、主要实验材料

树莓派4B主板、SD卡、USB电源、1k欧姆电阻、红色LED发光二极管、杜邦线

#### 3、实验步骤

*   连接电路
    
    发光二极管正极接上1kΩ电阻，两边连接杜邦线。将二极管正极插入GPIO引脚21(BCM编码，也就是物理引脚的40)，二极管负极接入GND(这里插入物理引脚39)
    
*   树莓派编写程序代码
    
    在这之前先检查Python是否安装了 `RPI.GPIO` 模块，Python2已经预装，Python3需要手动安装
    
    ```
    sudo apt-get update
    sudo apt-get install python3-rpi.gpio
    ```
    
    创建`pi.py` 文件,写入：
    
    ```python
    import RPi.GPIO as GPIO
    import time
    #注意BOARD和BCM编码的不同，这里设置的是BCM编码
    GPIO.setmode(GPIO.BCM)
    GPIO.setup(21, GPIO.OUT)
    #闪5次
    for i in range(5):
      GPIO.output(21,GPIO.HIGH)
      time.sleep(1)
      GPIO.output(21,GPIO.LOW)
      time.sleep(1)
    #建议每次退出时都用cleanup设置GPIO引脚为低电平状态
    GPIO.cleanup()
    ```
    
*   运行py程序
    
    ```c
    sudo python pi.py
    ```
    
    观察到二极管灯一闪一闪既大功告成！
    

#### 4、总结

关于在过程中遇到的一些问题

*   python版本问题
    
    树莓派4B默认使用python2.7，我们可以将它删除更换为python3.7
    
    ```c
    //卸载python2.7
    sudo apt-get autoremove python2.7
    //链接python3.7
    sudo ln -s /usr/bin/python3.7 /usr/bin/python
    //链接pip3
    sudo ln -s /usr/bin/pip3 /usr/bin/pip
    ```
    
*   安装rpi.gpio库出现**Requirement already satisfied**问题
    

![image-20220314223217804](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220314223217804.png)

将安装的rpi模块直接指向文件目录

```python
  pip install --target=/usr/lib/python3/dist-packages rpi.gpio
```