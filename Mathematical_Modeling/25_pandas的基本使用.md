# 定义一个字典

```python
# 定义一个 DataFrame 对象
# 数据使用字典
# 也要列出列的名称以便于一一匹对
df = pd.DataFrame(
    {
        "id": [1001, 1002, 1003, 1004, 1005, 1006],
        "date": pd.date_range("20130102", periods=6),
        "city": ["Beijing", "SH", "guangzhou", "Shenzhen", "shanghai", "BEIJING"],
        "age": [23, 44, 54, 32, 34, 32],
        "category": ["100-A", "100-B", "110-A", "110-C", "210-A", "130-F"],
        "price": [1200, np.nan, 2133, 5433, np.nan, 4432],
    },
    columns=["id", "date", "city", "category", "age", "price"],
)
df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>shanghai</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
值得注意的是，`date` 数据可以按照时间戳的方式输出。

# 输出形状

```python
# 输出形状
df.shape
```

```shell
(6, 6)
```

# 输出信息

```python
# 输出信息
df.info
```

```shell
<bound method DataFrame.info of      id date       city category  age   price
0  1001  NaN    Beijing    100-A   23  1200.0
1  1002  NaN         SH    100-B   44     NaN
2  1003  NaN  guangzhou    110-A   54  2133.0
3  1004  NaN   Shenzhen    110-C   32  5433.0
4  1005  NaN   shanghai    210-A   34     NaN
5  1006  NaN    BEIJING    130-F   32  4432.0>
```

# 返回每行的下标和相应下标的值

```python
# 行索引的下标以及相应下标的值
row_index_name_1 = df.index
row_index_name_2 = df.index.values
print(row_index_name_1)
print(row_index_name_2)
```

```shell
RangeIndex(start=0, stop=6, step=1)
[0 1 2 3 4 5]
```

---

# 输出相应变量的值

```python
# 输出相应变量的值
df["city"].values
```

```shell
array(['Beijing', 'SH', 'guangzhou', 'Shenzhen', 'shanghai', 'BEIJING'],
      dtype=object)
```

---

# 输出变量的数据类型

```python
df.dtypes
```

```shell
id            int64
date         object
city         object
category     object
age           int64
price       float64
dtype: object
```

---

# 判断是否为空

```python
# 判断是否为空
df["price"].isnull()
```

```shell
0    False
1     True
2    False
3    False
4     True
5    False
Name: price, dtype: bool
```

---

# 去除重复值

```python
# 去除重复值
df["age"].unique()
```

```shell
array([23, 44, 54, 32, 34], dtype=int64)
```

---

# 筛选

```python
dataframe = pd.DataFrame({"a": [1, 2, 3], "b": ["aaa", "bbb", "ccc"]})

# 选取属性 "a" 中大于 1 的行：
dataframe[dataframe.a > 1]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>bbb</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ccc</td>
    </tr>
  </tbody>
</table>
---

```python
# 选取属性 "a" 中大于 1 的 "b" 列：
dataframe["b"][dataframe.a > 1]
```

```shell
1    bbb
2    ccc
Name: b, dtype: object
```

---

```python
# 选取属性 "b" 在 list 中的行
array = np.array(["aaa", "ccc"])
dataframe[dataframe.b.isin(array)]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>aaa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>ccc</td>
    </tr>
  </tbody>
</table>
---

```python
dataframe_1 = df.loc[df["city"] == "Beijing", ["age", "id"]]
dataframe_1
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>23</td>
      <td>1001</td>
    </tr>
  </tbody>
</table>
---

```python
dataframe_2 = df.loc[df["age"] < 34, ["city", "id"]]
dataframe_2
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Beijing</td>
      <td>1001</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Shenzhen</td>
      <td>1004</td>
    </tr>
    <tr>
      <th>5</th>
      <td>BEIJING</td>
      <td>1006</td>
    </tr>
  </tbody>
</table>
---

```python
dataframe_3 = df.loc[(df["age"] < 50) & (df["price"] < 6000), ["id", "city"]]
dataframe_3
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>Beijing</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>Shenzhen</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>BEIJING</td>
    </tr>
  </tbody>
</table>
---

# 填充空缺值

```python
# 填充空缺值，如果在原对象修改，则添加 inplace 参数，并置为 True，即 fillna(value=10, inplace=True)
new_df = df.fillna(value=10)
new_df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>shanghai</td>
      <td>210-A</td>
      <td>34</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
---

```python
# 均值填充
df["price"] = df["price"].fillna(value=df["price"].mean())
# 线性填充
df["price"] = df["price"].interpolate()
# 三次样条填充
df["price"] = df["price"].interpolate()
```

# 字符处理方法

```python
df["city"] = df["city"].str.upper()
df
```

# 数据处理方法

## 改变类型

```python
df["price"] = df["price"].astype(int)
df
```

## 去重

```python
df["city"] = df["city"].drop_duplicates()
df
```

## 合并

```python
df_inner = pd.merge(df, df1, how="inner")
df_left = pd.merge(df, df1, how="left")
df_right = pd.merge(df, df1, how="right")
df_outer = pd.merge(df, df1, how="outer")
```

`left` 和 `right` 代表着左合并与右合并，一个以左边数据集为基础，一个以右边数据集为基础。

`inner` 只对两个所共用的进行保存，`outer` 是并集。

## 设置下标

```python
df_inner.set_index("id")
```

将数据中的某一列设置为下标。

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>pay</th>
      <th>m-point</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1001</th>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200</td>
      <td>male</td>
      <td>Y</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1002</th>
      <td>2013-01-03</td>
      <td>Sh</td>
      <td>100-B</td>
      <td>44</td>
      <td>3299</td>
      <td>female</td>
      <td>N</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1003</th>
      <td>2013-01-04</td>
      <td>Guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133</td>
      <td>male</td>
      <td>Y</td>
      <td>20</td>
    </tr>
    <tr>
      <th>1004</th>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433</td>
      <td>female</td>
      <td>Y</td>
      <td>40</td>
    </tr>
    <tr>
      <th>1005</th>
      <td>2013-01-06</td>
      <td>Shanghai</td>
      <td>210-A</td>
      <td>34</td>
      <td>3299</td>
      <td>male</td>
      <td>N</td>
      <td>40</td>
    </tr>
    <tr>
      <th>1006</th>
      <td>2013-01-07</td>
      <td>NaN</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432</td>
      <td>female</td>
      <td>Y</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
## 排序

### 根据某一列进行排序

```python
df_inner.sort_values(by=["price"])
```

### 根据下标进行排序

```python
df_inner = df_inner.sort_index()
df_inner
```

# 分组

```python
data = {
    'Product': ['A', 'B', 'A', 'B', 'A', 'B'],
    'Sales': [100, 200, 150, 300, 120, 250],
    'Quantity': [10, 15, 12, 20, 8, 18]
}

df = pd.DataFrame(data)

grouped = df.groupby("Product")
total_quantities = grouped["Quantity"].sum()
total_quantities
```

```shell
Product
A    30
B    53
Name: Quantity, dtype: int64
```

# 抽样

在 _Pandas_ 中，`sample()` 方法用于从 `DataFrame` 中随机抽取指定数量或比例的样本。下面是该方法的基本语法和一些示例用法：

1. **基本语法**：

```python
df.sample(n=None, frac=None, replace=False, random_state=None)
```

- `n`：要抽取的样本数量。
- `frac`：要抽取的样本比例，取值范围为 [0, 1]。
- `replace`：是否允许重复抽样，默认为 False。
- `random_state`：随机种子，用于复现随机抽样结果。

参数 `n` 和 `frac` 二选一，如果同时指定了，`frac` 会被忽略。

2. **示例**：

假设有一个包含学生信息的 `DataFrame`：

```python
import pandas as pd

data = {
    'StudentID': [1, 2, 3, 4, 5],
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'Age': [20, 21, 22, 23, 24]
}

df = pd.DataFrame(data)
print(df)
```

输出：

```
   StudentID     Name  Age
0          1    Alice   20
1          2      Bob   21
2          3  Charlie   22
3          4    David   23
4          5      Eva   24
```

现在，我们可以使用 `sample()` 方法随机抽取一定数量的样本：

```python
# 抽取 2 个样本
sample1 = df.sample(n=2)
print(sample1)
```

输出：

```
   StudentID     Name  Age
3          4    David   23
2          3  Charlie   22
```

你也可以指定抽样的比例：

```python
# 抽取 50% 的样本
sample2 = df.sample(frac=0.5)
print(sample2)
```

输出：

```
   StudentID   Name  Age
0          1  Alice   20
4          5    Eva   24
```

如果你希望允许重复抽样，可以设置 `replace=True`：

```python
# 允许重复抽样，抽取 3 个样本
sample3 = df.sample(n=3, replace=True)
print(sample3)
```

输出：

```
   StudentID     Name  Age
2          3  Charlie   22
3          4    David   23
2          3  Charlie   22
```

# 生成统计摘要

```python
# 生成统计摘要
df.describe()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Sales</th>
      <th>Quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>186.666667</td>
      <td>13.833333</td>
    </tr>
    <tr>
      <th>std</th>
      <td>77.888810</td>
      <td>4.665476</td>
    </tr>
    <tr>
      <th>min</th>
      <td>100.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>127.500000</td>
      <td>10.500000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>175.000000</td>
      <td>13.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>237.500000</td>
      <td>17.250000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>300.000000</td>
      <td>20.000000</td>
    </tr>
  </tbody>
</table>
