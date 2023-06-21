---
title: Python学习笔记二()
tags:
  - Numpy
  - Python
id: '766'
categories:
  - - 学习笔记
  - - 文章
date: 2022-03-19 18:02:09
---

### 1.、随机数

*   随机数
    
    ```python
    >>>import random
    >>>random.seed()#设置随机数种子
    >>>random.randint(1,100)#生成1-100随机整数
    >>>random.uniform(-100,100)#生成-100到100的随机浮点数
    >>>round(random.uniform(-100,100),2)#保留两位小数
    ```
    
*   随机数组
    
    ```python
    >>>import numpy as np
    >>>rand = np.random
    >>>rand.randint(1,100,(3,6))#生成3个数组，每个数组由6个1-100的随机整数组成
    >>>rand.rand(4)*100#生成1个数组，由4个0-100的随机浮点数组成。rand()返回值为[0,1]
    ```
    

### 2、Numpy扩展包
<!-- more -->
ndarray(代表一种特殊的数据结构—n维数组)是numpy的灵魂，Python中的列表和元组内存大、计算时间长。使用数组能优化这些缺点。

###### 2.1 ndarray创建

```python
#导入numpy包
import numpy as np
#第一种方法,和内置函数range类似,速度更快，返回的是ndarray类型
myarray1 = np.arange(1,10,2)#[1 3 5 7 9]
#第二种方法
myarray2 = np.array([1,3,5,7,9])#[1 3 5 7 9]
#第三种方法,返回几行几列填满相同一个元素的数组
myarray3 = np.full((2,3),6)#[[6 6 6] [6 6 6]]
#第四种，生成随机数组，可用np.random.RandomState()指定随机种子数
myarray4 = np.random.randint(0,10,(2,3))#[[1 3 8] [8 4 2]]
```

###### 2.2 主要特征

*   shape(形状)
    
    指定数组是一维数组或多维数组，shape=(2,15)代表2行15列的数组，且`shape=`可以省略。
    
*   dtype(元素类型)
    
    指定数组元素类型，dtype=np.int代表元素为numpy中的int型。ndarray的类型比Python自带的多
    

###### 2.3 切片/读取

*   规则切片操作
    
    ndarray数组支持像list那样的规则切片操作
    
*   不规则切片操作
    
    也可以使用Fancy Indexing方法进行不规则切片操作, 格式为 `[[index1,index2,index3...]]`, 即方括号内嵌套方括号。比如我想取数组的索引为第1个、第2个、第4个、第5个。
    
    ```python
    fancy_index = np.arange(0,100)
    fancy_index[[1,2,4,5]]
    #[1 2 4 5]
    ```
    

###### 2.4 多维数组的切片

以二维数组为例

```python
#切片的使用，[行进行切片,列进行切片] 即[start:stop:step,start:stop:step]
#获取所有行
print(a[:,:])
#结果：
#[[ 0  1  2]
# [ 3  4  5]
# [ 6  7  8]
# [ 9 10 11]]

#获取所有行，部分列 {所有行，第二列}
print(a[:,1])
#[ 1  4  7 10]

#获取所有行，部分列 {所有行，第一、二列}
print(a[:,0:2])
#[[ 0  1] [ 3  4] [ 6  7] [ 9 10]]

#获取部分行，部分列 {行的奇数行，列的第一、第二列}
print(a[::2,0:2])
#[[0 1] [6 7]]

#使用坐标获取[(1,2),(2,0)]，(1,2)均为行号，(2,0)均为列号，且1和2会自动结合，2和0会自动结合,3和结合
print(a[(1,2,3),(2,0,1)])
#结果：[5 6 10]
```

###### 2.5浅拷贝和深拷贝

浅拷贝是指拷贝过来的是—引用。

```python
import numpy as np
arr1 = np.array([1,2,3,'A','B','C'])
arr2 = np.arange(1,10,2)

arr1 = arr2 #此处arr1对arr2进行了浅拷贝，二者引用相同存储空间
arr1[1] = 0
print(arr1,arr2)
#[1 0 5 7 9] [1 0 5 7 9]
#对arr1的修改也会影响arr2 的值
```

深拷贝是指重新申请一个存储空间存放，使用的是 `copy()`方法。

```python
import numpy as np
arr1 = np.array([1,2,3,'A','B','C'])
arr2 = np.arange(1,10,2)

arr1 = arr2.copy() #此处arr1对arr2进行了深拷贝，arr1重新获得一个新的存储空间存放相同的arr2元素
arr1[1] = 0
print(arr1,arr2)
#[1 0 5 7 9] [1 3 5 7 9]
#arr2并未发生变化
```

###### 2.6形状和重构

查看形状，用 属性`shape`

```python
import numpy as np
arr1 = np.array([1,2,3,'A','B','C'])
arr2 = np.random.randint(0,10,(2,3))

print(arr1.shape)
print(arr2.shape)
#(6,)
#(2,3)
```

`reshape()`, 返回一个符合形状的多维数组，不改变原数组

```python
import numpy as np
arr = np.arange(0,20)
arr2 = arr.reshape(4,5)
print(arr2)
#[[ 0  1  2  3  4]
# [ 5  6  7  8  9]
# [10 11 12 13 14]
# [15 16 17 18 19]]
```

**注意**：重构的数组必须是n × m = 原数组总元素个数，否则会报错

![重构的数组必须是n × m = 原数组总元素个数](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/%E9%87%8D%E6%9E%84%E7%9A%84%E6%95%B0%E7%BB%84%E5%BF%85%E9%A1%BB%E6%98%AFn%20%C3%97%20m%20=%20%E5%8E%9F%E6%95%B0%E7%BB%84%E6%80%BB%E5%85%83%E7%B4%A0%E4%B8%AA%E6%95%B0.png)

`resize()` 改变原数组

```python
import numpy as np
arr = np.arange(0,20)
arr.resize(4,5)
print(arr)
#[[ 0  1  2  3  4]
# [ 5  6  7  8  9]
# [10 11 12 13 14]
# [15 16 17 18 19]]
```

`flatten()` 将多维数组变为一维数组，返回值为一维数组，不改变原数组

`swapaxes()` 进行轴调换，实现转置矩阵。不改变数组本身

```python
import numpy as np
arr = np.arange(0,20)
arr.resize(4,5)
arr_swap = arr.swapaxes(0,1)
print(arr,arr_swap)
#[[ 0  1  2  3  4]
# [ 5  6  7  8  9]
# [10 11 12 13 14]
# [15 16 17 18 19]]
#[[ 0  5 10 15]
# [ 1  6 11 16]
# [ 2  7 12 17]
# [ 3  8 13 18]
# [ 4  9 14 19]]
```

###### 2.7 属性计算

计算数组的秩，用 `ndim` 属性

```python
import numpy as np
arr = np.arange(0,20)
arr.resize(4,5)
print(arr.ndim)
# 2
```

计算元素个数，用 `size` 属性

```python
import numpy as np
arr = np.arange(0,20)
print(arr.size)
# 20
```

数组的乘法，不改变原数组

```python
import numpy as np
arr = np.arange(0,20)
arr1 = arr*10
print(arr1)
#[  0  10  20  30  40  50  60  70  80  90 100 110 120 130 #140 150 160 170
# 180 190]
```

###### 2.8 插入和删除

`np.delete(ArrayName,index)` 删除特定元素, 不改变数组`np.insert(ArrayName,index,value)` 插入特定元素, 不改变数组

```python
import numpy as np
arr = np.arange(0,10,2)
###########delete#####
arr1 =np.delete(arr,2)
print(arr,arr1)
#[0 2 4 6 8] [0 2 6 8]
###########insert#######
arr2 = np.insert(arr,1,999)
print(arr,arr2)
#[0 2 4 6 8] [  0 999   2   4   6   8]
```

###### 2.9 缺失值和广播规则

###### 其它

*   数组内排序：
    
    使用 `np.sort(ArrayNameaxis )` ,返回排序结果不改变数组本身。
    
    其中参数axis则表示按行排序还是按列排序。