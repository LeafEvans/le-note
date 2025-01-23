# `string` 类

## `string` 容器基本概念

C++ 标准库定义了一种 `string` 类，定义在头文件 `<string>`。

`string` 和 C 风格字符串对比：

- `char *` 是一个指针，`string` 是一个类；
- `string` 封装了 `char *`，管理这个字符串，是一个 `char` 型的容器；
- `string` 封装了很多实用的成员方法：查找（`find`）、拷贝（`copy`）、删除（`delete`）、替换（`replace`）、插入（`insert`）。
- 不用考虑内存释放和越界，`string` 管理 `char *` 所分配的内存。每一次 `string` 的复制，取值都由 `string` 类负责维护，不用担心复制越界和取值越界等。

## `string` 容器常用操作

### `string` 构造函数

```cpp
string();                   // 初始化一个空字符串
string(const string& str);  // 使用一个 string 对象初始化另一个 string 对象
string(const char *s);      // 使用字符串 s 初始化
string(int n, char c);      // 使用 n 个字符串 c 初始化
```

### `string` 基本赋值操作

```cpp
string& operator=(const char *s);      // char * 类型字符串赋值给当前的字符串 
string& operator=(const string &s);    // 把字符串 s 赋给当前的字符串
string& operator=(char c);             // 字符赋值给当前的字符串
string& assign(const char *s);         // 把字符串 s 赋给当前的字符串
string& assign(const char *s, int n);  // 把字符串 s 的前 n 个字符赋给当前的字符串
string& assign(const string& s);       // 把字符串 s 赋给当前的字符串
string& assign(int n, char c);         // 用 n 个字符 c 赋给当前字符串
string& assign(const string &s, int start, int n);  // 将字符串 s 从 start 开始的 n 个字符赋给字符串
```

### `string` 存取字符操作

```cpp
char& operator[](int n);
char& at(int n);
```

> 注意：`[]` 不会抛出异常，而 `at` 越界会抛出异常。

### `string` 拼接操作

```cpp
string& operator+=(const string& str);            
string& operator+=(const char *str);
string& operator+=(const char c);
string& append(const char *s);         // 把字符串 s 连接到当前字符串结尾
string& append(const char *s, int n);  // 把字符串 s 的前 n 个字符连接到当前字符串结尾
string& append(const string &s);  
string& append(const string &s, int pos, int n);  // 把字符串 s 中从 pos 开始的 n 个字符连接到当前字符串结尾
string& append(int n, char c);  // 在当前字符串结尾添加 n 个字符 c
```

### `string` 查找与替换

```cpp
int find(const string& str, int pos = 0) const;  // 查找 str 第一次出现位置，从 pos 开始找
int find(const char *s, int pos = 0) const;      // 查找 s 第一次出现位置，从 pos 开始找
int find(const char *s, int pos, int n) const;   // 从 pos 位置查找 s 的前 n 个字符第一次位置
int find(const char c, int pos = 0) const;       // 查找字符 c 第一次出现位置
int rfind(const char *s, int pos = npos) const;  // 查找 s 最后一次出现位置
int rfind(const char *s, int pos = npos) const;  // 从 pos 查找 s 的前 n 个字符最后一个位置
int rfind(const char *s, int pos, int n) const;  // 查找字符 c 最后一次出现位置
int rfind(const char c, int pos = 0) const;      // 替换从 pos 开始 n 个字符为字符串 str
string& replace(int pos, int n, const string& str);  // 替换从 pos 开始 n 个字符为 str
string& replace(int pos, int n, const char* s);      // 替换从 pos 开始 n 个字符为 s
```

### `string` 比较操作

```c++  
int compare(const string &s) const;  // 与字符串 s 比较
int compare(const char *s) const;    // 与字符串 s 比较
```

### `string` 子串

```cpp
string substr(int pos = 0, int n = npos) const;  // 返回从 pos 开始的 n 个字符组成的字符串
```

### `string` 插入和删除操作

```cpp
string& insert(int pos, const char* s);      // 插入字符串
string& insert(int pos, const string& str);  // 插入字符串
string& insert(int pos, int n, char c);      // 在指定位置插入 n 个字符 c
string& erase(int pos, int n = npos);        // 删除从 pos 开始的 n 个字符
```

### `string` 和 C 风格字符串转换

```cpp
// string 转 char *
string str = "itcast";
const char *cstr = str.c_str();
// char * 转 string
char *s = "itcast";
string str(s);
```

在 C++ 中存在一个从 `const char` 转换为 `string` 的隐式类型转换，却不存在一个从 `string` 对象到 C 风格字符串的自动类型转换。对于 `string` 类型字符串，可以通过 `c_str()` 函数返回 `string` 对象对应的 C 风格字符串。通常，程序员在整个程序中应该坚持使用 `string` 对象，直到必须将内容转换为 `char *` 才将其转换为 C 风格字符串。

> 注意：为了修改 `string` 字符串的内容，下标操作符 `[]` 和 `at` 都会返回字符的引用；但当字符串的内容被重新分配后，可能发生错误。