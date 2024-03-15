---
title: C++实现LRU算法
date: 2024-03-15 17:58:25
tags: C++
thumbnail: https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/LRU.png
cover: https://redamancy9189.oss-cn-beijing.aliyuncs.com/Icons/%E7%AB%99%E7%82%B9logo.png
---

# C++实现LRU算法

## 描述

原题：[146. LRU 缓存 - 力扣（LeetCode）](https://leetcode.cn/problems/lru-cache/description/)

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

**示例**

> 输入
> ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
> [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
> 输出
> [null, null, null, 1, null, -1, null, -1, 3, 4]

**解释**

```C++
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

**提示**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`

## 核心代码

本题的思想在于要在O(1)时间复杂度上完成 `get` 和 `put` 操作。对于 `get` 则使用哈希表(C++的 `std::unordered_map`)，对于 `put` 则使用双链表，同时采用伪头尾指针记录，即可在O(1)时间内完成操作。理解结构图很重要，数据结构图如下：

![LRU](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/LRU.png)

### 哈希表

```C++
std::unordered_map<int, LinkListNode*> _hashmap; // 记录哈希表
```

### 双链表

```C++
// 链表节点
typedef struct LinkListNode {
    int key, value;
    LinkListNode* prev;
    LinkListNode* next;
    LinkListNode():key(0),value(0),prev(nullptr),next(nullptr){};
    LinkListNode(int key, int value):key(key),value(value),prev(nullptr),next(nullptr){}
}LinkListNode;

```

初始化

```c++
// 初始化双链表
    LRUCache(int capacity):_capacity(capacity), _size(0) {
        _dummyHead = new LinkListNode();
        _dummyTail = new LinkListNode();
        _dummyHead->next = _dummyTail;
        _dummyTail->prev = _dummyHead;
    }
```

### ⭐获取cache

```C++
// 按关键字获取cache
    int get(int key) {
        if(!_hashmap.count(key)) { // cache 不命中，返回-1
            return -1;
        }
        // LRU核心：cache 命中，返回值，同时更新节点到头部
        LinkListNode* node = _hashmap[key];
        moveToHead(node);
        return node->value;
    }
```

### ⭐加入cahce

```C++
 // 向cache加入节点
    void put(int key, int value) {
        if(!_hashmap.count(key)) { // cache 不命中
            LinkListNode* node = new LinkListNode(key, value);
            // 是否移除尾部元素
            if(_size >= _capacity){   // 当前元素大于等于cache容量，移除尾元素
                // !重点!:不要忘记去除哈希表内key, value
                _hashmap.erase(_dummyTail->prev->key);  
                // 去除尾节点，即LRU核心：替换掉最近最少使用的元素
                removeNode(_dummyTail->prev);  
            } else {
                // size加1，对应下面添加新节点
                _size++;
            }
            // 记录新的节点到哈希表
            _hashmap[key] = node;
            // 将新的节点增加到双链表头节点  
            addToHead(node);
        } else {
            // 取出cache命中的节点，更新新值
            LinkListNode* node = _hashmap[key];
            node->value = value;
            // LRU核心，被使用则更新节点到双链表头部
            moveToHead(node);
    	}
    }
```

剩下的 **删除指定节点** 、**移动指定节点到头节点**、**增加节点到头节点** 实现如下

## 完整代码

```C++
// 链表节点
typedef struct LinkListNode {
    int key, value;
    LinkListNode* prev;
    LinkListNode* next;
    LinkListNode():key(0),value(0),prev(nullptr),next(nullptr){};
    LinkListNode(int key, int value):key(key),value(value),prev(nullptr),next(nullptr){}
}LinkListNode;

class LRUCache {
private:
    // 记录cache 总容量
    int _capacity;
    // 记录当前 cache 占用个数
    int _size; 
    std::unordered_map<int, LinkListNode*> _hashmap; // 记录哈希表
    LinkListNode* _dummyHead;
    LinkListNode* _dummyTail;
public:
    // 初始化双链表
    LRUCache(int capacity):_capacity(capacity), _size(0) {
        _dummyHead = new LinkListNode();
        _dummyTail = new LinkListNode();
        _dummyHead->next = _dummyTail;
        _dummyTail->prev = _dummyHead;
    }
    // 按关键字获取cache
    int get(int key) {
        if(!_hashmap.count(key)) { // cache 不命中，返回-1
            return -1;
        }
        // LRU核心：cache 命中，返回值，同时更新节点到头部
        LinkListNode* node = _hashmap[key];
        moveToHead(node);
        return node->value;
    }
    // 向cache加入节点
    void put(int key, int value) {
        if(!_hashmap.count(key)) { // cache 不命中
            LinkListNode* node = new LinkListNode(key, value);
            // 是否移除尾部元素
            if(_size >= _capacity){   // 当前元素大于等于cache容量，移除尾元素
                // 不要忘记去除哈希表内key, value
                _hashmap.erase(_dummyTail->prev->key);  
                // 去除尾节点，即LRU核心：替换掉最近最少使用的元素
                removeNode(_dummyTail->prev);  
            } else {
                // size加1，对应下面添加新节点
                _size++;
            }
            // 记录新的节点到哈希表
            _hashmap[key] = node;
            // 将新的节点增加到双链表头节点  
            addToHead(node);
        } else {
            // 取出cache命中的节点，更新新值
            LinkListNode* node = _hashmap[key];
            node->value = value;
            // LRU核心，被使用则更新节点到双链表头部
            moveToHead(node);
    }
    }
    // 删除节点并释放内存
    void removeNode(LinkListNode* node) {
        if(node == nullptr) return;
        node->prev->next = node->next;
        node->next->prev = node->prev;
        delete node;
    }
    // 移动节点到双链表头部
    void moveToHead(LinkListNode* node) {
        if(node == nullptr) return;
        node->next->prev = node->prev;
        node->prev->next = node->next;

        node->next = _dummyHead->next;
        node->prev = _dummyHead;
        _dummyHead->next->prev = node;
        _dummyHead->next = node;
    }
    // 增加节点到双链表头部
    void addToHead(LinkListNode* node) {
        if(node == nullptr) return;

        node->next = _dummyHead->next;
        node->prev = _dummyHead;
        _dummyHead->next->prev = node;
        _dummyHead->next = node;
    }
};

```

## 运行结果

![image-20240315172555097](https://redamancy9189.oss-cn-beijing.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20240315172555097.png)