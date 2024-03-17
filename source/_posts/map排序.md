---
title: C++ map排序
date: 2024-03-17 21:40:24
tags: C++

---
# C++ map按key或按value排序

### map按key排序

（1）map默认按照 key 从小到大排序

```c++
map<string,int> hash;
//等价于 map<string,int, less<string>> hash;
```

（2）map按照 key 从大到小排序

```c++
map<string,int, greater<string> > hash;
```

### map按value值排序

按 value 值排序没有直接的方法，但我们可以把 map 存到 vector 中，再对 vector 进行自定义排序

重写 vector 的 cmp 函数

```C++
bool cmp(pair<string,int> a, pair<string, int> b) {
    return a.second < b.second;//从小到大排序
}
```

把 map 存到 vector 中进行排序

```C++
map<string,int> m;
m["a"] = 2;
m["b"] = 3;
m["c"] = 1;
vector<pair<string,int> > vec(m.begin(),m.end());
/*
 *vec(m.begin(),m.end()代表用map的所有元素创建vector
*/
sort(vec.begin(),vec.end(),cmp);
```

