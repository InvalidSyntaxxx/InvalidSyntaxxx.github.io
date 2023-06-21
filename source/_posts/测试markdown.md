---
title: Python学习笔记(一)
tags:
  - Python
id: '253'
categories:
  - - 专业技术
  - - 文章
date: 2022-03-01 22:58:11
---

### 1.Python缩进

```python
elements = ["Tom","Jack","Anne"]
for elem in elements:
    print("Hello",elem) #缩进表示一个代码块
    print("I love you",elem)
print("Bye My",elements)#不缩进代表一个独立的语句，不在for循环体内
```

### 2.基本类型
<!-- more -->
*   整数
    
*   浮点数
    
    `1.23e9`可以表示很大的浮点数
    
*   字符串
    
    可以使用`"`或者`'`括起来的，字符串内可用转义字符`\`标识
    
*   布尔值
    
    `True`和 `False` 布尔值还可以用布尔运算，有三种：`and，or， not`与之对应的逻辑是数理逻辑上的与或非
    
*   空值
    
    是Python一个特殊值，用 `None`表示
    
*   列表、字典等
    

#### 2.1变量

大小写英文、数字和`_`的组合，且不能用数字开头

#### 2.2常量

Python里通常用全大写变量名表示常量，但不强制

#### 2.3字符串编码

*   Python3采用Unicode编码，常见计算机系统内存统一采用Unicode编码，当需要保存到硬盘或者传输时，就转化为UTF-8编码，反之转化回Unicode。
    
*   Python对于 `bytes` 类型的数据用带 `b` 前缀的单引号或双引号表示：
    
    ```python
    x = b'ABC'
    ```
    
*   字符串可用encode( )方法可以编码为指定的bytes
    
    ```python
    >>> 'ABC'.encode('ascii')
    b'ABC'
    >>> '中文'.encode('utf-8')
    b'\xe4\xb8\xad\xe6\x96\x87'
    >>> '中文'.encode('ascii')
    ```
    
*   len( )函数计算str字符数
    
    ```python
    >>> print(len("你好"))
    2
    >>> print(len("nihao"))
    5
    ```
    
    如果是计算bytes，则是字节数
    
    ```python
    >>> len(b'ABC')
    3
    >>> len(b'\xe4\xb8\xad\xe6\x96\x87')
    6
    >>> len('中文'.encode('utf-8'))
    6
    ```
    
    坚持使用UTF-8编码避免乱码问题
    

### 3.格式化

```python
>>> print("hello ,%s,I'm %d years old,my score is %f"%("jack",18,95.00))
hello ,jack,I'm 18 years old,my score is 95.000000
```

另外一种是str.format( )方法，不推荐

### 4.list 和tuple

*   list是一种有序的集合，可以增删，
    
    ```python
    elements = ["A",123,["C","D"],True]
    elements.append("abcd")
    elements.insert(2,("2","3"))
    elements.pop(0)
    print(elements)
    print(elements[-3][0])#如果是elements[-2][0]会报TypeErrorw
    ###########输出结果自行理解
    [123, ('2', '3'), ['C', 'D'], True, 'abcd']
    C
    ```
    
*   tuple一旦初始化不能修改，没有append( ), insert( )这种方法，能使用-1...表示索引
    
    tuple用 `()` 表示(注意和数学符号意义上的区别，通常用 `,` 如（ 1,）加以区分)
    
    tuple里面若有指向别的元素如list，指向的list不可改变，但是list指向的可以改变
    
    ```python
    tuple = ("A", "B", ["C","D"])#tuple里面的元素["C","D"]是一个list，list可以改变
    ```
    

### 5.dict和set

*   dict

### 6.Python小结

列表list \[\]、元组tuple ()、字典 dict {key: value}、无序不重复集合 set (list\[\])

### 7.函数

*   #### 参数
    
    *   位置参数
    
    平时常用的按照顺序进行传参的参数
    
    ```python
    def power(x,n):
        temp = 1
        if n<0:
            for t in range(n,0):#t表示n到-1的整数
                temp = temp/x
        elif n>0:
            for t in range(0,n):#range(n,m)表示返回n到m-1的整数list
                temp = temp*x
        return temp
    print("power(5,3):",power(5,3))#power(5,3): 125
    print("power(5,-2):",power(5,-2))#power(5,-2): 0.04
    ```
    
    *   默认参数
    
    默认参数在后面，默认参数必须指向不变对象，不然下次调用时默认参数内容就变了
    
    ```python
    def enroll(name,age,city="北京",sex="保密"):
        print("My name is ",name,",I`m ",age)
        print("I`m come from ",city,",my sex is",sex)
    enroll("李华",23,sex = "女")
    #My name is  李华 ,I`m  23 
    #I`m come from  北京 ,my sex is 女   
    enroll("张三",18)
    #My name is  张三 ,I`m  18
    #I`m come from  北京 ,my sex is 保密  
    enroll("李四",45,"上海","男")
    #My name is  李四 ,I`m  45
    #I`m come from  上海 ,my sex is 男  
    ```
    
    *   可变参数
    
    在参数前加一个 `*` ，参数接收到的可以是一个tuple，也可以是任意个参数包括0个
    
    对于已有的tuple和list，在前面加一个 `*` 就可变成可变参数传入函数
    
    ```python
    def calc(*numbers):
        sum = 0
        for number in numbers:
            sum = sum + number
        return sum
    nums1 = [1,3,5,7]
    nums2 = (1,3,5,7)
    print(calc(1,3,5,7)) #16
    print(calc(*nums2))  #16
    print(calc(*nums1))  #16
    ```
    
    *   关键字参数
    
    允许传入0至多个参数，在函数内部自动组装为一个dict
    
    ```python
    def info(name, age, **more):
        print(name," ",age," ",more)
        if "city" in more:
            pass 
        if "addr" in more:
            print("my city:",more.get("addr")) #more相当于一个字典
    
    info("李华",12,addr="钦州市",sex = "男")
    #结果都为：李华   12   {'addr': '钦州市', 'sex': '男'} my city: 钦州市
    extra = { "addr":"钦州市","sex":"男"}
    info("李华",12,**extra)#这里相当于把extra的一份拷贝给函数info的关键字参数more
    ```
    
    小结：对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。
    
*   #### 递归
    
*   #### 元信息
    
    ```python
    #为函数添加元数据,描述函数的参数类型和返回类型
    def add(x:int, y:int) -> int:
      return x+y
    ```
    
    元信息只是为了帮助源码阅读。python解释器不会对这些注解添加任何的语义。即使返回类型不一致，程序也不会出错，因为它们不会被类型检查。
    

### 8.高级特性

*   #### 切片
    
    `:` 用来取list或tuple的部分元素
    
    ```python
    L = list(range(100,200))
    print(L[:3])#取list前三个元素
    print(L[1:3])#取list中索引值为1到索引值为3的元素
    print(L[-20:-10])#取倒数第十到倒数第二十也就是180-190（不包括190）之间的十个整数
    print(L[:10:2])#前十个元素，每隔2个元素取一个，结果为100,102,104,106,108
    print(L[::-1])#实现切片逆序
    ```
    
*   #### 迭代
    
    1、 通过`for...in`来完成迭代，不仅可以在 `tuple` 、 `list` 和 `dict` 上进行迭代，还可以作用于可迭代对象（可通过`collections.abc` 模块的 `Iterable` 类型判断)
    
    ```python
    >>>isinstance('abc',Iterable)#str是否能迭代
    True
    >>>isinstance(123,Iterable)#整数是否能迭代
    False
    ```
    
    2、实现Java下标循环
    
    ```python
    >>>for i,x in enumerate(['A','B','C']):
    >>>    print(i,x)
    0 A
    1 B
    2 C
    ```
    
*   #### 列表生成式
    
    1.  列表生成式可以嵌套一层至多层循环
        
        ```python
        #列表生成式直接一行代码生成list
        >>>print([x*x for x in range(1,5)])
        [1,4,9,16]
        #还可以使用像for循环那样的两个变量即以上
        >>>dictItem = {'A':'a','B':'b','C':'c'}
        >>>print([a+'='+b for a,b in dictItem.items])
        ['A=a','B=b','C=c']
        #for循环后还能添加if判断条件，注意！！这是筛选条件，和下面的if...else不一样
        >>>print([x*x for x in range(1,10) if x%2==0])
        [4, 16, 36, 64]
        #还可以添加多层循环
        >>>print([a+b for a in ['1','2','3'] for b in ['A','B','C']])
        ['1A', '1B', '1C', '2A', '2B', '2C', '3A', '3B', '3C']
        ```
        
    2.  `if...else`的使用(使用方法为在for循环前加上if...else)
        
        ```python
        >>>dictItem = {'A':2,'B':'b','C':'c','D':3}
        >>>print([a+'='+b if isinstance(b,str) else a+'='+str(b) for a,b in dictItem.items()])
        ['A=2', 'B=b', 'C=c','D=3']
        ```
        
*   #### 生成器(Generator)
    
    `next()`、`yelid`、`StopIteration`
    
    generator函数返回一个generator对象
    

### 9\. 高阶函数

*   #### `map()` 、 `reduce()` 和 `filter()`
    
    1.  `map()`
        
        接收一个函数和Iterator，将`函数作用到所有Iterator元素`上并将结果返回到新的Iterator，最终结果返回新的Iterator。
        
        ```python
        >>> def f(x):
             return x * x
        >>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
        >>> list(r)#最后要用list函数将Iterator转换为list
        [1, 4, 9, 16, 25, 36, 49, 64, 81]
        ```
        
    2.  `reduce`
        
        接收一个函数和一个序列，该函数必须接收`两个参数`，`reduce` 负责将`函数结果`继续和 `序列下一个元素` 作为两个参数继续执行下一次函数，直到后序无元素，最终结果为`计算结果`
        
        ```python
        >>>def add(x,y):
        >>>    return x+y
        >>>reduce(add,[1,3,5,7])
        16
        ```
        
    
    思考题：利用`map`和`reduce`编写一个`str2float`函数，把字符串`'123.456'`转换成浮点数`123.456`
    
    3.  `filter`
        
        接收一个函数和一个序列，将 `函数作用于每个元素` ，然后根据返回值是 `True` 还是 `False` 决定保留或者丢弃元素。
        
        ```python
        >>>def not_empty(s):
        >>>    return s and s.strip()
        >>>print(list(filter(not_empty,['A',None,'B','']))
        ['A', 'B']
        ```
        
*   #### `sorted()`
    
    `sorted()` 可以对list进行排序，也是一个高阶函数，可以接收一个 `key` 函数自定义排序顺序
    
    ```python
    >>>sorted([9,-1,2,4,-5],key=abs)
    [-1, 2, 4, -5, 9]
    ```
    
    `sorted()`函数负责将key函数`返回的结果`进行排序，并按对应关系返回list中的元素
    
    此外，要实现反向排序还可以传入 `reverse` 的值为 `True`
    
    ```python
    >>>print(sorted([9,-1,2,4,-5],key=abs,reverse=True))
    [9, -5, 4, 2, -1]
    ```
    
    `sorted()` 返回结果为list
    

### 10\. 函数式编程

*   #### 返回函数
    
    1.  一个函数可以返回值，也可以`返回函数`
        
        ```python
        def  lazy_sum(L):
          def sum(L):
                 result = 0
                 for x in L:
                     result = result + x
                 return result
          return sum #注意返回sum和sum()的区别！！！
        test = lazy_sum([1,2,3,4,5])
        print(test())#返回的是sum，则需要调用test();如果返回的是sum(),则直接调用test
        ```
        
        ​ 上述例子，如果函数内只是返回`函数名`而不是返回执行函数，则为 `闭包运算` ；此外，返回闭包时，要注意返回函数不能引用任何循环变量或者后续会发生变化的变量。
        
        ![](https://s3.bmp.ovh/imgs/2022/02/ed7aed0162d048c1.png)
        
    2.  `nonlocal`
        
        声明该变量不是当前函数的局部变量
        
*   匿名函数(lambda)
    
    关键字 `lambda` 参数 `x,y` : 函数返回值 `x+y`
    
    ```python
    lambda x,y: x+y 
    ```
    
    多数时候配合高阶函数 `map()`、`filter()`、`reduce()`、`sorted()` 使用
    
*   装饰器(decorator)
    
    在代码运行期间动态增加功能的方式。(动态语言的函数和类的定义，是在运行时动态创建的。)
    
    要借助Python的 `@` 语法，把decorator放在函数 `定义处`：
    
    ```python
    import time
    
    def log(func):
      def wrapper(*args,**kw):
          print('call %s()'%func.__name__)
          return func(*args,**kw)
      return wrapper
    @log   #相当于执行 now = log(now)
    def now():
      print(time.strftime("%Y-%m-%d %H:%M:%S"))
    ```
    
*   偏函数
    
    当函数的参数个数太多，需要简化时，使用`functools.partial`可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单
    
    ```python
    >>> import functools
    >>> int2 = functools.partial(int, base=2)
    >>> int2('1000000')
    64
    >>> int2('1010101')
    85
    ```
    

### 11\. 模块

*   作用域
    
    `_xxx` 或者 `_xxx_` 表示的 `函数` 或者 `变量` 一般为非公开的(private)，不应该被直接引用(但依然可以在其它模块强制引用)。
    
    ```python
    #这是工具类.py模块
    >>>import datetime
    >>>import time
    >>>def showtime():
    >>>    print(time.strftime("%Y-%m-%d %H:%M:%S"))
    >>>__t = time.strftime("%Y")
    >>>def __showyear():
    >>>    print(__t)
    
    #这是test.py模块
    >>>from 工具类 import showtime,__showyear,__t
    >>>showtime()
    >>>print(__t)#不推荐直接使用其它模块的private变量或函数，即使可以正常使用
    >>>__showyear()
    2022-02-26 18:36:49
    2022
    2022
    ```
    
    只有外部需要的函数或者变量才定义为public。
    
*   安装第三方模块
    
    包管理工具 `pip`
    

### 12.面向对象编程

*   类和实例及其访问权限
    
    ```python
    #新建Student类，并继承Object类
    class Student(object):
      #__init__方法初始化对象，相当于java的构造子
      #定义类时，self代表"实例的引用"
      def __init__(self, name, gender):
      #__xxx习惯上设置访问权限为private，但还是可以强制访问
          self.__name = name
          self.__gender = gender
      #设置对象里的方法
      def get_gender(self):
          return self.__gender
    
      def set_gender(self,s):
          if s =='male'or'female':
              print(self.__gender)
              self.__gender = s
              print(self.__gender)
          else:
              raise ValueError('值错啦')
    # 测试:
    bart = Student('Bart', 'male')
    if bart.get_gender() != 'male':
      print('测试失败!')
    else:
      bart.set_gender('female')
      if bart.get_gender() != 'female':
          print('测试失败!')
      else:
          print('测试成功!')
    ```
    
*   方法
    
    Python中的方法分为三种：实例方法、类方法、静态方法。
    
    *   实例方法
    
    参数里面有 `self` ，如：
    
    ```python
    def set_self(self,name):
        self.__name = name
    ```
    
    *   类方法
    
    前面带有装饰器 `@classmethod` ,且参数里面有 `cls` ，如：
    
    ```python
    @classmethod
    def class_func(cls):
        cls.name = "我是类名"
        print("my name is %s"%(cls.name))
    ```
    
    *   静态方法
    
    形参中没有 `cls` 和 `self`，甚至没有参数，如：
    
    ```python
    import random
    @staticmethod
    def static_func():
        #返回0-9的随机数
        randomNum = random.randint(0,9)
        return randomNum
    ```
    
    类方法和静态方法都可以通过类名和实例名调用
    
*   获取对象信息
    
    1.  `type()方法`
        
        ```python
        >>> type(123)==int
        True
        >>> type('abc')==type('123')
        True
        >>> type('abc')==str
        True
        ```
        
        该方法返回对应的Class类型
        
    2.  `id()方法`
        
        ```python
        >>>str1 = 'test'
        >>>str2 = "test"
        >>>str3 = str1
        >>>int1 = 123
        >>>list1 = [1,2,'3']
        >>>tuple1 = (1,2,3,[1,2,'3'])
        >>>total =['test',str2,str3,int1,list1,tuple1]
        >>>print(list(id(i) for i in total[:3] if >>>type(i)==str))
        >>>print(list(id(i) for i in total))
        [2408464217968, 2408464217968, 2408464217968]
        [2408464217968, 2408464217968, 2408464217968, 2408455690416, 2408507421120, 2408507410448]
        ```
        
        从输出结果可以看出程序运行时str1、str2、str3的id值是一样的
        
    3.  `isinstance()方法`
        
        如果有继承关系：Object ->Animal ->Dog ->Husky
        
        那么则
        
        ```python
        a = Animal()
        b = Dog()
        h = Husky()
        >>>isinstance(h,Husky)
        True
        >>>isinstance(h,Animal)
        True
        ```
        
        `isinstance()`还可以判断其类型及其子类,一般情况下优先使用`isinstance()`判断类型
        
        `isinstance()`还可以判断是否为某些变量其一
        
        ```python
        >>> isinstance([1, 2, 3], (list, tuple))
        True
        >>> isinstance((1, 2, 3), (list, tuple))
        True
        ```
        
    4.  `__doc__属性`
        
        ```python
        >>>print(len.__doc__)
        Return the number of items in a container. 
        ```
        
        用于查看某对象支持的方法或属性清单
        
    
    此外还有 `help()`方法，可以帮助显示方法或属性信息
    
*   类属性
    
    直接定义在类中，区别于实例属性self.xxx
    
*   动态绑定
    
    动态绑定允许我们在程序运行的过程中动态给class加上功能
    
    ```python
    class Student(object):
      def __init__(self,name):
          self.__name__ = name
    stu1 = Student('张三')
    #动态绑定给实例绑定一个属性
    stu1.sex = 'male'
    print(stu1.sex)
    # 动态绑定给实例绑定一个方法
    def  set_name(self,name):
      self.__name__ = name
    from types import MethodType
    stu1.set_name = MethodType(set_name, stu1) 
    stu1.set_name('李四')
    print(stu1.__name__)
    ```
    
    为了限制类外动态绑定属性或方法，可以定义在定义类时用`__slots__`变量，来限制该class实例能添加的属性。
    
    ```python
    class Student(object):
      __slots__ = ('name','__name__')
      def __init__(self,name):
          self.__name__ = name
    ```
    
    此时如果向Student类添加sex属性则会报AttributeError![image-20220312211908812](https://gitee.com/wang-yuanzhao/personal-drawing-bed/raw/master/images/image-20220312211908812.png)
    
    **注意**：`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用
    
    练习：利用`@property`给一个`Screen`对象加上`width`和`height`属性，以及一个只读属性`resolution`
    
    ```python
    class Screen(object):
      @property
      def  width(self):
          return self.__width__ 
      @width.setter
      def width(self,size):
          if isinstance(size,(int,float))==False:
              raise TypeError('请输入正确的int或其它数值类型')
          if size < 0 or size > 2048:
              raise ValueError('请输入正确范围的数')
          self.__width__ = size
      @property
      def  height(self):
          return self.__height__
      @height.setter
      def height(self,size):
          if isinstance(size,(int,float))==False:
             raise TypeError('请输入正确的int或其它数值类型')
          if size < 0 or size > 2048:
             raise ValueError('请输入正确范围的数')
          self.__height__ = size   
      @property
      def  resolution(self):
          return self.__width__*self.__height__
    ```
    
*   多重继承
    
    通过多重继承，一个子类就可以同时获得多个父类的所有功能
    
    ```python
    class Dog(Mammal,Runnable):
      def __init__():
          pass
    ```
    
    `MixIn` 用于分清主父类和额外功能(类似于Java的继承单一父类和多继承接口)
    
    ```python
    class Dog(Mammal,RunnableMixIn):
       def __init__():
          pass
    ```
    
*   `__str__`
    
    Python里面的 `__str__` 相当于Java 的 `toString()`方法
    
    ```python
    class Student(object):
      __slots__ = ('name','__name__')
      def __init__(self,name):
          self.__name__ = name
      def __str__(self) -> str:
          return 'my name is%s '%(self.__name__)
    stu1 = Student('张三')
    print(stu1)  #结果为：my name is 张三
    ```
    
    同样的还有 `__getattr__`、 `__call__`、 `__iter__`、 `__getitem__` 等
    
    可根据需要前往[官网文档](https://docs.python.org/3/tutorial/index.html)查阅相关资料，不再赘述。
    

### 13\. 异常、错误、调试

*   错误与错误
    
    高级语言通常都内置了一套`try...except...finally...`的错误处理机制，Python也不例外
    
    ```python
    try:
      #可能发生异常的语句
    except IOError as e :
      #发生IOError异常时要执行的语句
      print("An EOF error occurred.")
      raise e
    except (Ex2,Ex3):
      #发生异常Ex2或Ex3时要执行的语句
    except Exception:
      #发生其它异常时要执行的语句
    else:
      #无异常时要执行的语句
    finally:
      #无论有没有异常都要执行的语句
      #如文件资源、数据库、图形句柄资源的释放
    ```
    
*   调试
    
    有三种方式：`Python Debugger`、`%xmode`、`assert`
    
    [Python学习笔记(二)](https://wangwangyz.site/?p=766 "Python学习笔记(二)")