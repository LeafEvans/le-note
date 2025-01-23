# `map`、`multimap` 容器

## `map`、`multimap` 基本概念

`map` 的特性是所有元素都会**根据元素的键值自动排序**。`map`所有的元素都是 `pair`，同时拥有实值和键值，`pair` 的第一元素被视为键值，第二元素被视为实值。`map` 不允许两个元素**有相同的键值**。

> 注意：不可以通过 `map` 的迭代器改变 `map` 的**键值**，因为 `map` 的键值关系到 `map` 元素的排列规则，任意改变 `map` 键值将会严重破坏 `map` 组织；如果想要修改元素的**实值**，可行。

`map` 和 `list` 拥有相同的某些性质，当对它的容器元素进行新增操作或者删除操作时，操作之前的所有迭代器，在操作完成之后依然有效，当然被删除的那个元素的迭代器必然是个例外。`multimap` 和 `map` 的操作类似，唯一区别 `multimap` 键值可重复。`map` 和  `multimap` 都是以红黑树为底层实现机制。

<img src="https://leafalice-image.oss-cn-hangzhou.aliyuncs.com/img/image-20250123030328455.png" alt="image-20250123030328455" style="zoom: 67%;" />

`map` 容器每个元素都是“键值-实值”成对存储，自动根据键值排序，键值不能重复，不能修改。

### `map`、`multimap` 常用 API

### `map` 构造函数

```c++
map<T1, T2> mapT;    // map 默认构造函数
map(const map &mp);  // 拷贝构造函数
```

### `map` 赋值操作

```cpp
map& operator=(const map &mp);
swap(mp);
```

### `map` 大小操作

```c++
map.insert(...);  // 往容器内插入元素，返回对组
map<int, string> mapStu;
mapStu.insert(pair<int, string>(3, "小徐"));
mapStu.insert(make_pair(-1, "校长"));
mapStu.insert(map<int, string>::value_type(1, "小李"));
mapStu[3] = "小刘";
mapStu[5] = "小王";
mapStu.emplace(2, "小张");
```

### `map` 删除操作

```cpp
clear();          // 删除所有元素
erase(pos);       // 删除 pos 迭代器所指的元素，返回下一个元素的迭代器
erase(beg, end);  // 删除区间 [beg, end) 的所有元素，返回下一个元素的迭代器
erase(keyElem);   // 删除容器中 key 为 keyElem 的对组
```

### `map` 查找操作

```cpp
find(key);
count(keyElem);
lower_bound(keyElem);
upper_bound(keyElem);
equal_range(keyElem);
```

