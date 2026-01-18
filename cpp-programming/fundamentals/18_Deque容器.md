# `deque` 容器

## `deque` 容器基本概念

`vector` 容器是单向开口的连续内存空间，`deque` 则是一种双向开口的连续线性空间。所谓的双向开口，意思是可以在头尾两端分别做元素的插入和删除操作。当然，`vector` 容器也可以在头尾两端插入元素，但是在其头部操作效率奇差，无法被接受。

<img src="../../images/cpp-programming/image_20250122_201900.webp" style="zoom:50%;" />

`deque` 容器和 `vector` 容器最大的差异：

- `deque` 允许使用常数项时间对头端进行元素的插入和删除操作
- `deque` **没有容量的概念**，因为它是动态的以**分段连续空间**组合而成，随时可以增加一段新的空间并链接起来；换句话说，像 `vector` 那样，“旧空间不足而重新配置一块更大空间，然后复制元素，再释放旧空间“这样的事情在 `deque` 身上是**不会发生的**；也因此，`deque` 没有必须要提供所谓的空间保留（`reserve`）功能

虽然 `deque` 容器也提供了 Random Access Iterator，但是它的迭代器并不是普通的指针，其复杂度和 `vector` **不是一个量级**，这当然影响各个运算的层面。因此，除非有必要，我们应该<u>尽可能的使用 `vector`，而不是 `deque`</u>。对 `deque` 进行的排序操作，为了最高效率，可将 `deque` 先完整的复制到一个 `vector` 中，对 `vector` 容器进行排序，再复制回 `deque`。

## `deque` 容器实现原理

`deque` 容器是连续的空间，至少逻辑上看来如此。连续现行空间总是令我们联想到 `array` 和 `vector`，`array` 无法成长，`vector` 虽可成长却只能向尾端成长，而且其成长其实是一个假象，事实上：

1. 申请更大空间
2. 原数据复制新空间
3. 释放原空间

如果不是 `vector` 每次配置新的空间时都**留有余裕**，其成长假象所带来的代价是非常昂贵的。

`deque` 是**由一段一段的定量的连续空间构成**。一旦有必要在 `deque` 前端或者尾端增加新的空间，便配置一段连续定量的空间，**串接**在 `deque` 的头端或者尾端。`deque` 最大的工作就是维护这些分段连续的内存空间的整体性的假象，并提供随机存取的接口，避开了重新配置空间、复制、释放的轮回，代价就是**复杂的迭代器架构**。

既然 `deque` 是分段连续内存空间，那么就必须有中央控制，维持整体连续的假象，数据结构的设计及迭代器的前进后退操作颇为繁琐。`deque` 代码的实现远比 `vector` 或 `list` 都多得多。`deque` 采取一块所谓的 map（不是 STL 的 `map` 容器）作为主控，这里所谓的 map 是一小块连续的内存空间，其中每一个元素（此处成为一个结点）都是一个指针，指向另一段连续性内存空间，称作缓冲区。缓冲区才是 `deque` 的存储空间的主体。

<img src="../../images/cpp-programming/image_20250122_204600.webp" style="zoom:50%;" />

## `deque` 常用 API

### `deque` 构造函数

```cpp
deque<T> deqT;            // 默认构造函数
deque(beg, end);          // 拷贝函数将 [beg, end) 区间中的元素拷贝给本身
deque(n, elem);           // 构造函数将 n 个 elem 拷贝给本身
deque(const deque &deq);  // 拷贝构造函数
```

### `deque` 赋值操作

```c++
assign(beg, end);                    // 将 [beg, end) 区间中的数据赋值给本身
assign(n, elem);                     // 将 n 个 elem 拷贝赋值给本身
deque& operator=(const deque &deq);  // 重载等号操作符
swap(deq);                           // 将 deq 与本身的元素互换
```

### `deque` 大小操作

```cpp
deque.size();             // 返回容器中元素个数
deque.empty();            // 判断容器是否为空
deque.resize(num);        // 重新指定容器的长度为 num，若容器变长，则以默认值填充新位置，如果容器变短，则末尾超出容器长度的元素被删除
deque.resize(num, elem);  // 重新指定容器的长度为 num，若容器变长，则以 elem 值填充新位置，如果容器变短，则末尾超出容器长度的元素被删除
```

### `deque` 双端插入和删除操作

```cpp
push_back(elem);  // 在容器尾部添加一个数据
push_back(elem);  // 在容器头部添加一个数据
pop_back();       // 删除容器最后一个数据
pop_front();      // 删除容器第一个数据
```

### `deque` 插入操作

```cpp
insert(pos, elem);      // 在 pos 位置插入一个 elem 元素，返回数据的位置
insert(pos, n, elem);   // 在 pos 位置插入 n 个 elem 元素，无返回值
insert(pos, beg, end);  // 在 pos 位置插入 [beg, end) 区间的数据，无返回值
```

### `deque` 删除操作

```cpp
clear();          // 移除容器的所有数据
erase(beg, end);  // 删除 [beg, end) 区间的数据，返回下一个数据的位置
erase(pos);       // 删除 pos 位置的数据，返回下一个数据的位置
```
