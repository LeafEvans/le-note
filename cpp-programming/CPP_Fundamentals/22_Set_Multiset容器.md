# `set`、`multiset` 容器

## `set`、`multiset` 容器基本概念

`set` 的特性是所有元素都会根据元素的键值**自动排序**，`set` 的元素**既是键值又是实值**。

<img src="https://leafalice-image.oss-cn-hangzhou.aliyuncs.com/img/image-20250123021109835.png" alt="image-20250123021109835" style="zoom:67%;" />

> 注意：`set` 不允许两个元素有相同的键值。

`multiset` 特性及用法与 `set` 完全一致，唯一的区别在于它**允许键值重复**。

## `set` 常用 API

### `set` 构造函数

```c++
set<T> st;           
mulitset<T> mst;     
set(const set& st);
```

### `set` 赋值操作

```cpp
set& operator=(const set& st);
swap(st);
```

### `set` 大小操作

```c++
size();
empty();
```

### `set` 插入和删除操作

```cpp
insert(elem);     // 在容器中插入元素
clear();          // 清除所有元素
erase(pos);       // 删除 pos 指向的元素，返回下一个元素的迭代器
erase(beg, end);  // 删除区间 [beg, end) 区间的所有元素，返回下一个元素的迭代器
erase(elem);      // 删除容器中值为 elem 的元素
```

### `set` 查找操作

```c++
find(key);             // 查找键 key 是否存在；若存在则返回该键的元素的迭代器；若不存在，则返回 end()
count(key);            // 统计键为 key 的元素个数
lower_bound(keyElem);  // 返回第一个 key >= keyElem 元素的迭代器
upper_bound(keyElem);  // 返回第一个 key > keyElem 元素的迭代器
equal_range(keyElem);  // 返回容器中 key 与 keyElem 相等的上下限的两个迭代器
```

