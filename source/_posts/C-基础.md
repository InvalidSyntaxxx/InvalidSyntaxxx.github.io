---
title: C++基础
date: 2024-03-12 18:55:12
tags:
---
# Bustub前置知识：C++

## STL

[C++ STL详解超全总结(快速入门STL)-CSDN博客](https://blog.csdn.net/qq_50285142/article/details/114026148)

**string**

```C++
#include <iostream>
#include <string> // 引入 string 头文件

int main () {
    string s1 = "123";
    string s2 = "456";
	string s = s1 + s2; // 支持 + 连接符
    
    string s;
	cin >> s; //读入字符串，遇空格，回车结束
    string s = "xing ma qi";
	char ch[] = s.c_str(); // 实现string向char数组的转换
}
```

| 函数                  | 意义                       |
| --------------------- | -------------------------- |
| s.size() 、s.length() | 字符串的字符个数           |
| s.max_size()          | string对象最多包含的字符数 |
| s.capacity()          | 同上？？？                 |

```c++
s.push_back()	//在末尾插入
s.push_back('a')	//末尾插入一个字符a
s.insert(pos,element)	//在pos位置插入element
s.insert(s.begin(),'1')	//在第一个位置插入1字符
s.append(str)	//在s字符串结尾添加str字符串
s.append("abc")
erase(iterator p)	//删除字符串中p所指的字符
erase(iterator first, iterator last)	//删除字符串中迭代器区间[first,last)上所有字符
erase(pos, len)	//删除字符串中从索引位置pos开始的len个字符
clear()	//删除字符串中所有字符

s.substr(pos,n)	// 截取从pos索引开始的n个字符

```

**map**：内部用**红黑树**实现，具有**自动排序**（按键从小到大）功能。

**unordered_map**：内部用**哈希表**实现，内部元素无序杂乱。

## C++11新功能

- auto关键字
- Lambda
- 智能指针(unique_ptr, shared_ptr, weak_ptr)
- 右值引用
- 元组

### auto关键字

1. 常用在循环中

   ```C++
   // Iterating using auto
   for (auto itr = mapOfStrs.begin(); itr != mapOfStrs.end(); ++itr)
   {
       std::cout << itr->first << "::" << itr->second << std::endl;
   }
   ```

2. auto类型确定后不可变

   ```C++
   auto x = 1;
   // This will cause a compile-time error because 'x' is of type int
   // x = "dummy";
   ```

3. 返回类型为 auto

   ```C++
   auto sum(int x, int y) -> int {
       return x + y;
   }
   
   // Calling the function that returns 'auto'
   auto value = sum(3, 5);
   ```

### 可变参数模板

可变参数模板是**可以接受任意数量参数的模板**。

log函数工作原理：

1. 它打印第一个参数。
2. 它以递归方式调用其余参数。

```C++
#include <iostream>

// Function to end the recursion of variadic template function
void log() {
    // This can be empty or used to print something that marks the end of output.
}

template<typename T, typename... Args>
void log(T first, Args... args) 
{
    std::cout << first;
    if constexpr (sizeof...(args) > 0)
    {
        std::cout << " , ";
        log(args...);
    }
    else
    {
        std::cout << std::endl; // New line for the last element
    }
}

int main()
{
    // Calling log() functio with 3 arguments
    log(1 , 4.3 , "Hello");

    // Calling log() functio with 4 arguments
    log('a', "test", 78L, 5);

    // Calling log() functio with 2 arguments
    log("sample", "test");

    return 0;
}
```

### delete关键字

`delete`关键字可以应用于函数，使它们不可调用

下面是删除函数的简单示例：

```c++
void someFunction（） = delete;
```

**隐式类型转换**可能很方便，但很危险。如果类型意外转换，它们可能会导致数据丢失或逻辑错误。删除特定的构造函数可以防止这些不需要的转换。例如：

```C++
// A constructor that might lead to implicit conversions
User(int userId, std::string userName);

// Creating an object with potentially problematic conversions
User obj4(5.5, "Riti");  // double to int
User obj5('a', "Riti");   // char to int
```

若要防止这些转换，请将这些构造函数声明为 deleted：

```C++
// Deleting constructors to prevent implicit conversions
User(double userId, std::string userName) = delete;
User(char userId, std::string userName) = delete;
```

在这里，我们将删除两个可能导致隐式转换的构造函数：

**限制在堆上创建对象**, 要确保仅在堆栈（而不是堆上）创建类的实例，您可以删除 new 运算符：

```c++
class User {
    // ...
    // Delete the new operator to prevent heap allocation
    void* operator new(size_t) = delete;
};
```

此行确保 new 运算符不能与 User 类一起使用。因此，以下代码将触发编译时错误

```c++
// Error: Use of deleted function 'static void* User::operator new(size_t)'
User* userPtr = new User(1, "Riti");
```



### std::bind

是一种通用工具，用于通过**将参数绑定到给定函数**来创建新的函数对象

```C++
#include <iostream>
#include <functional> // 包含这个头文件以使用 std::bind
using namespace std;
int add(int first, int second){
    return first + second;
}

int main(){
    auto add_func = std::bind(&add, std::placeholders::_1, std::placeholders::_2);
    cout << "result:" << add_func(1, 3) << endl;
    return 0;
}
```

### Lambda

```C++
[capture](parameters) mutable exception -> return_type {
    // function body
}
```

[capture]

- `[]`：未捕获任何内容。
- `[x, &y]`：`x`按值捕获，`y`按引用捕获。
- `[=]`：作用域中的所有变量都按值捕获。
- `[&]`：作用域中的所有变量都通过引用捕获。

三个示例

```c++
int y = 10;
auto lambda = [&y](int x) mutable {
    y = x * y; // 这是有效的，因为 y 是通过引用捕获的，并且 lambda 是 'mutable'
    std::cout << y << std::endl;
};
lambda(5); // 输出 50
```

```C++
#include <iostream>
#include <algorithm>

int main()
{
    int arr[] = {1, 2, 3, 4, 5};
    int discount = 50;

    // 按值捕获
    std::for_each(arr,
                  arr + sizeof(arr) / sizeof(int),
                  [=](int x) mutable {
                        std::cout << x << " ";
                        // 无法在此处修改“discount”，因为它是按值捕获的，除非使用“mutable”。
                        discount = 20;
                  });

    std::cout << std::endl;

    std::cout << "discount Vaue: " << discount << std::endl;
    return 0;
}
/* 输出：
1 2 3 4 5 
discount Vaue: 50
*/
```

### 列表初始化

用**花括号**初始化器列表初始化一个对象，其中对应构造函数接受一个 `std::initializer_list` 参数.

```C++
#include <iostream>
#include <vector>
#include <initializer_list>
 
template <class T>
struct S {
    std::vector<T> v;
    S(std::initializer_list<T> l) : v(l) {
         std::cout << "constructed with a " << l.size() << "-element list\n";
    }
    void append(std::initializer_list<T> l) {
        v.insert(v.end(), l.begin(), l.end());
    }
    std::pair<const T*, std::size_t> c_arr() const {
        return {&v[0], v.size()};  // 在 return 语句中复制列表初始化
                                   // 这不使用 std::initializer_list
    }
};
 
template <typename T>
void templated_fn(T) {}
 
int main()
{
    S<int> s = {1, 2, 3, 4, 5}; // 复制初始化
    s.append({6, 7, 8});      // 函数调用中的列表初始化
 
    std::cout << "The vector size is now " << s.c_arr().second << " ints:\n";
 
    for (auto n : s.v)
        std::cout << n << ' ';
    std::cout << '\n';
 
    std::cout << "Range-for over brace-init-list: \n";
 
    for (int x : {-1, -2, -3}) // auto 的规则令此带范围 for 工作
        std::cout << x << ' ';
    std::cout << '\n';
 
    auto al = {10, 11, 12};   // auto 的特殊规则
 
    std::cout << "The list bound to auto has size() = " << al.size() << '\n';
 
//    templated_fn({1, 2, 3}); // 编译错误！“ {1, 2, 3} ”不是表达式，
                             // 它无类型，故 T 无法推导
    templated_fn<std::initializer_list<int>>({1, 2, 3}); // OK
    templated_fn<std::vector<int>>({1, 2, 3});           // 也 OK
}
```



## 什么是左值什么是右值？

[理解 C/C++ 中的左值和右值 | nettee 的 blog](https://nettee.github.io/posts/2018/Understanding-lvalues-and-rvalues-in-C-and-C/)

**左值 (lvalue, locator value)** 表示占据<u>内存</u>中<u>某个可识别的位置</u> 的 <u> 对象</u>。

**右值 (rvalue)** 则使用排除法来定义。一个表达式不是 *左值* 就是 *右值* 。 那么，右值是一个 **不表示**内存中某个可识别位置的对象的表达式。

### 左/右值转换

```C++
int a = 1;
int b = 2;
int c = a + b; // a, b, c是左值，但在 "+" 运算时转换为了右值。即左值隐式转化为右值。
```

**但是**，右值不能转换为左值！不过右值可以显式转化为左值

```C++
int arr[] = {1, 2};
int *p = &arr[0];
*(p+1) = 10; // p+1 为右值，但指针符号 * 将 p+1 转换为左值。
```

### ⭐右值引用

右值引用，这个功能自C++11起才可用。**移动语义**是C++11新增的重要功能，其重点是对右值的操作。右值可以看作程序运行中的临时结果，右值引用可以避免复制提高效率。`&&`用法示例：

```cpp
#include <iostream>
struct Foo {
  ~Foo() {std::cout << "destruction" << std::endl;}
};

Foo FooFactory() {
  return Foo();
}

int main() {
  std::cout << "before copy constructor..." << std::endl;
  Foo foo1 = FooFactory();
  std::cout << "after copy constructor..." << std::endl << std::endl;
  // 引用右值，避免生成新对象
  Foo&& foo2 = FooFactory();
  std::cout << "life time ends!" << std::endl << std::endl;

  return 0;
}
```

用`clang`编译器编译上述代码， 运行结果如下：

```coq
before copy constructor...
destruction
destruction
after copy constructor...

destruction
life time ends!

destruction
destruction
```

从输出结果看，第二种写法少了一次`destruction`输出，<u>特别是这里调用了额外的一对构造器/析构器，用来创建和销毁一个临时的对象</u>。这意味着通过右值引用(`&&`)，`foo2`直接引用`FooFactory`返回的对象，**避免了对象复制**。

### 移动语义

[认识C++移动语义与右值引用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/347977300)

**移动语义的“移动”，意味着把某对象持有的资源或内容转移给另一个对象。**

```cpp
// vec_red = vec_orange;            // vec_orange被绑定到const std::vector<int> &类型
vec_red = std::move(vec_orange);    // vec_orange被绑定到std::vector<int> &&类型
```

**vec_orange**是一个左值（它有名字），不经过`std::move()`的转型则会绑定到一个左值引用；而`std::move()`则把它转为一个右值引用，从而匹配到移动赋值运算符函数。

例子：`std::unique_ptr` 简易实现

```C++
template <typename T>
class my_unique_ptr
{
    T* ptr_;
public:
    // 移动构造函数和移动赋值函数
    my_unique_ptr(my_unique_ptr&& other): ptr_(std::exchange(other.ptr_, nullptr)){}
    my_unique_ptr& operator=(my_unique_ptr&& rhs)
    {
        if (this != &rhs) {
           if (ptr_) {  // 释放当前拥有的指针
               delete ptr_;
           }
           ptr_ = std::exchange(rhs.ptr_, nullptr); // 夺取rhs的指针
        }
    	return *this;
    }
    // 禁止复制构造函数和复制赋值函数
    my_unique_ptr(const my_unique_ptr&) = delete;
    my_unique_ptr& operator=(const my_unique_ptr&) = delete;
}

```

### noexcept

`noexcept` 是一个异常说明符，用于告诉编译器一个函数不会抛出任何异常。

> 如上图所示那般，老的元素是被拷贝到新的内存空间中的。是的，classes容器确实使用的是拷贝构造函数。那么此时我们会想到，既然classes容器已经不需要之前的内存中的数据了，那么将老数据放到新的内存空间中应该使用移动语义，而非拷贝操作。
>
> 那么为什么classes容器没有使用移动语义呢？
>
> 此时，我们需要提及一个概念，即"强异常保证（strong exception guarantee）"。所谓强异常保证，即当我们调用一个函数时，如果发生了异常，那么应用程序的状态能够回滚到函数调用之前：
>
> ![img](https://pic1.zhimg.com/80/v2-497f3dddeca16e784dc74de20887bec4_720w.webp)
>
> 
>
> 那么强异常保证和决定使用移动语义或拷贝操作又有什么关系呢？
>
> 这是因为容器的push_back函数是具备强异常保证的，也就是说，当push_back函数在执行操作的过程中（由于内存不足需要申请新的内存、将老的元素放到新内存中等），如果发生了异常（内存空间不足无法申请等），push_back函数需要确保应用程序的状态能够回滚到调用它之前。以上面的例子来说，当第2次执行`classes.push_back(A);`时，如果发生了异常，应用程序的状态会回滚到第1次执行`classes.push_back(A);`之后，即classes容器中只有一个元素。
>
> 由于我们的移动构造函数没有使用noexcept说明符，也就是我们没有保证移动构造函数不会抛出异常。因此，为了确保强异常保证，就只能使用拷贝构造函数了。那么拷贝构造函数同样没有保证不会抛出异常，为什么就能用呢？这是因为拷贝构造函数执行之后，被拷贝对象的原始数据是不会丢失的。因此，即使发生异常需要回滚，那些已经被拷贝的对象仍然完整且有效。但移动语义就不同了，被移动对象的原始数据是会被清除的，因此如果发生异常，那些已经被移动的对象的数据就没有了，找不回来了，也就无法完成状态回滚了。

###  总结

- **移动语义让程序员在复制对象时有所选择。**
- **右值引用是C++语法层面表示移动语义的机制。**
- **`std::move()`没有移动任何东西，只是把一个表达式转型（cast）为右值。**
- **尽量让具有移动语义的函数`noexcept`**



## C++多线程

[C++并发编程（C++11到C++17）-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/721543?utm_content=g_1000224569)

C++11提供如下4种语义的互斥量（mutex） ：

1. std::mutex，独占的互斥量，不能递归使用。
2. std::timed_mutex，带超时的独占互斥量，不能递归使用。
3. std::recursive_mutex，递归互斥量，不带超时功能。
4. std::recursive_timed_mutex，带超时的递归互斥量。

```cpp
#include <mutex>
#include <iostream>
#include <thread>

#define TRY_MUTEX       0
#define MY_MUTEX        1

volatile int counter(0); // non-atomic counter

std::mutex mtx;

void increases10k()
{
        for(int i=0;i<10000;i++)
        {
#if TRY_MUTEX
                if(mtx.try_lock())
                {
                        ++counter;
                        mtx.unlock();
                }
#elif MY_MUTEX
                mtx.lock();
                ++counter;
                mtx.unlock();
#endif
        }

}

int main(int argc,char **argv)
{
        std::thread threads[10];
        for(int i=0;i<10;i++)
        {
                threads[i]=std::thread(increases10k);
        }
        for(auto& th:threads)
                th.join();
        std::cout << " successful increases of the counter " << counter <<std::endl;
        return 0;

}
```

编译：

```bash
 g++ -o mutex_test mutex_test.c -std=c++11 -lpthread
```

执行效果：

```bash
# try_lock
$ ./mutex_test 
 successful increases of the counter 53612
# lock
$ vim mutex_test.c
 successful increases of the counter 100000
```

可以看到try_lock只是尝试加锁，不管是否成功都不阻塞，而lock如果加锁失败会一直阻塞直到加锁成功。

| API         | C++标准 | 说明                                   |
| ----------- | ------- | -------------------------------------- |
| lock_guard  | C++11   | 实现严格基于作用域的互斥体所有权包装器 |
| unique_lock | C++11   | 实现可移动的互斥体所有权包装器         |
| shared_lock | C++14   | 实现可移动的共享互斥体所有权封装器     |
| scoped_lock | C++17   | 用于多个互斥体的免死锁 RAII 封装器     |

| 锁定策略    | C++标准 | 说明                                                |
| ----------- | ------- | --------------------------------------------------- |
| defer_lock  | C++11   | 类型为 `defer_lock_t`，不获得互斥的所有权           |
| try_to_lock | C++11   | 类型为`try_to_lock_t`，尝试获得互斥的所有权而不阻塞 |
| adopt_lock  | C++11   | 类型为`adopt_lock_t`，假设调用方已拥有互斥的所有权  |

上面的几个类（`lock_guard`，`unique_lock`，`shared_lock`，`scoped_lock`）都使用了一个叫做RAII的编程技巧。

![image-20240309204057829](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20240309204057829.png)

### future

| API           | C++标准 | 说明                                              |
| ------------- | ------- | ------------------------------------------------- |
| async         | C++11   | 异步运行一个函数，并返回保有其结果的`std::future` |
| future        | C++11   | 等待被异步设置的值                                |
| packaged_task | C++11   | 打包一个函数，存储其返回值以进行异步获取          |
| promise       | C++11   | 存储一个值以进行异步获取                          |
| shared_future | C++11   | 等待被异步设置的值（可能为其他 future 所引用）    |

可以在并发环境中使用的工具，它们都位于`<future>`头文件中。

`std::future`可以看成存储器，存储一个未来返回值。先在主线程内创建一个promise对象，从promise对象中获得future对象；再将promise引用传递给任务线程，在任务线程中对promise进行`set_value`，主线程可通过future获得结果。

`std::future`提供了一个重要方法就是`.get()`，这将阻塞主线程，直到future就绪。注意：`.get()`方法只能调用一次。

此外，`std::future`不支持拷贝，支持移动构造。c++提供的另一个类`std::shared_future`支持拷贝。

可以通过下面三个方式来获得`std::future`。

- `std::promise`的get_future函数
- `std::packaged_task`的get_future函数
- `std::async` 函数

### async

`std::async`的第一个参数：`std::launch::deferred`【延迟调用】和`std::launch::async`【强制创建一个线程】。

1. `std::launch::deferred`:
   表示线程入口函数调用被延迟到`std::future`对象调用`wait()`或者`get()`函数 调用才执行。
   如果`wait()`和`get()`**没有调用**，则不会创建新线程，也不执行函数；
   如果调用`wait()`和`get()`，实际上**也不会创建新线程**，而是在主线程上继续执行；
2. `std::launch::async`:
   表示强制这个异步任务在 **新线程**上执行，在调用`std::async()`函数的时候就开始创建线程。
3. `std::launch::deferred|std::launch::async`:
   这里的“|”表示或者。如果没有给出launch参数，默认采用该种方式。
   操作系统会自行评估选择async or defer，如果系统资源紧张，则采用defer，就不会创建新线程。避免创建线程过长，导致崩溃。

嘶，async默认的launch方式将由操作系统决定，这样好处是不会因为开辟线程太多而崩溃，但坏处是这种不确定性会带来问题。std::async在使用时不仅要注意launch的不确定性，还有一个坑：**async返回的future对象的析构是异步的**。

### ⭐总结

1. **互斥量**（Mutex）：
   作用：互斥量用于实现临界区的互斥访问，确保在同一时刻只有一个线程可以访问被互斥锁保护的共享资源。

  特点和使用：使用 std::mutex 或其变体（如 std::recursive_mutex）来创建互斥锁。通过 std::lock_guard 或 std::unique_lock 来管理互斥锁的锁定和解锁。适用于需要保护共享资源免受并发访问的场景。

  ```c++
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;

void example_function() {
    std::lock_guard<std::mutex> lock(mtx);
    // 互斥区，对共享资源的访问受到保护
    // ...
}  // 锁在此处自动释放
  ```

2. **条件变量**（Condition Variable）

  作用：条件变量用于实现线程之间的协同操作，允许一个线程等待某个特定条件的发生，而其他线程可以在条件满足时通知等待的线程。

  特点和使用：使用 std::condition_variable 创建条件变量。与互斥锁一起使用，通常需要 std::unique_lock 进行锁的管理。适用于线程之间需要等待某个条件满足才能继续执行的情况。

  ```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool condition = false;

void producer() {
    std::lock_guard<std::mutex> lock(mtx);
    condition = true;
    cv.notify_one();
}

void consumer() {
    std::unique_lock<std::mutex> lock(mtx);
    cv.wait(lock, []{ return condition; });
    // 条件满足后继续执行
}

int main() {
    std::thread producer_thread(producer);
    std::thread consumer_thread(consumer);

    producer_thread.join();
    consumer_thread.join();

    return 0;
}
  ```

3. **期望**（std::future 和 std::promise）：

  作用：期望用于异步编程，允许一个线程获取另一个线程的异步任务的结果。

  特点和使用：使用 std::future 表示异步任务的未来结果，使用 std::promise 设置异步任务的值。
  通过 std::async 或 std::packaged_task 创建异步任务。
  适用于在一个线程中执行任务，而另一个线程等待任务的结果。

  ```c++
#include <iostream>
#include <future>

int add(int a, int b) {
    return a + b;
}

int main() {
    std::promise<int> promise_result;
    std::future<int> future_result = promise_result.get_future();

    std::thread([&](std::promise<int>& p) {
        p.set_value(add(1, 2));
    }, std::ref(promise_result)).detach();

    int result = future_result.get();
    std::cout << "Result: " << result << std::endl;

    return 0;
}
  ```

**总结**

- 互斥量用于保护共享资源，防止并发访问。
- 条件变量用于线程之间的协同操作，等待某个条件满足后继续执行。
- 期望用于异步编程，允许一个线程获取另一个线程的异步任务的结果。

## 类型转换
