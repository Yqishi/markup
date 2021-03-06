# Pandas

Pandas的名称来自于面板数据（panel data）和Python数据分析（data analysis）。

Pandas是一个强大的分析结构化数据的工具集，基于NumPy构建，提供了 **高级数据结构** 和 **数据操作工具**，它是使Python成为强大而高效的数据分析环境的重要因素之一。

- 一个强大的分析和操作大型结构化数据集所需的工具集
- 基础是NumPy，提供了高性能矩阵的运算
- 提供了大量能够快速便捷地处理数据的函数和方法
- 应用于数据挖掘，数据分析
- 提供数据清洗功能

<http://pandas.pydata.org>

## 数据结构

Pandas有两个最主要也是最重要的数据结构： **Series** 和 **DataFrame**

### Series

Series是一种类似于一维数组的 **对象**，由一组数据（各种NumPy数据类型）以及一组与之对应的索引（数据标签）组成。

- 类似一维数组的对象
- 由数据和索引组成
  - 索引(index)在左，数据(values)在右
  - 索引是自动创建的

#### 创建

```python
# 通过list创建
ser_obj = pd.Series(range(10))

# 通过ndarry创建
ser_obj = pd.Series(np.arange(10))

# 通过dict创建
ser_obj3 = pd.Series({"age": 18, "id": 1001, "name": "itcast"})

# 对象命名和索引命名
ser_obj4.name = "Stu"
ser_obj4.index.name = "Info"

# 通过index参数指定行索引创建
ser_obj3 = pd.Series(data=range(-3, 3), index=list("ABCDEF"), dtype=np.float64, name="测试数据")
```

#### 查看

```python
# 查看名字
对象名：ser_obj.name
对象索引名：ser_obj.index.name

# 查看对象索引数据
ser_obj3.index

# 查看对象值
ser_obj3.values

# 查看索引对应的值
ser_obj.label1
ser_obj["label1"]
ser_obj[pos]
```

### DataFrame

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同类型的值。DataFrame既有行索引也有列索引，它可以被看做是由Series组成的字典（共用同一个索引），数据是以二维结构存放的。

- 类似多维数组/表格数据 (如，excel, R中的data.frame)
- 每列数据可以是不同的类型
- 索引包括列索引和行索引

#### 创建

```python
# 通过嵌套列表/ndarray创建
import numpy as np
arr = np.random.rand(3, 4)
df_pbj1 = pd.DataFrame(arr)


# 通过dict构建DataFrame
dict_data = {'A': 1, 
             'B': pd.Timestamp('20170426'),
             'C': pd.Series(1, index=list(range(4)),dtype='float32'),
             'D': np.array([3] * 4,dtype='int32'),
             'E': ["Python","Java","C++","C"], }
# 字典的键对应列索引，同一列的数据类型必须相同，不同列之间数据类型可以不同
# 如果的单个数据，会自动填充到最多的个数，如果是多个数据，必须个数相同
df_obj3 = pd.DataFrame(dict_data)

# 增加列数据
df_obj[new_col_idx] = data

# 创建行索引与列索引
df_pbj2 = pd.DataFrame(arr, index= ['A', 'B', 'C'], columns=['a', 'b', 'c', 'd'])
```

#### 查看

```python
# 产看DataFrame对象的详细信息
df_obj.info()

# 查看行索引和列索引对象
df_obj.index
df_obj.columns

# 通过索引获取列数据
df_obj[col_idx]
df_obj.col_idx
```

## 索引操作

### 查看数据

#### Series

```
# 指定行索引名
ser_obj1 = pd.Series(range(10, 15), index= list("ABCDE"))

# 取单个数据
ser_obj[‘label’]
ser_obj.label
ser_obj[pos]

# 取连续的多个数据(切片索引)
# 按索引名切片操作时，是包含结束位的
ser_obj[‘label1’: ’label3’]
ser_obj[2:4]

# 不连续多个数据
ser_obj[[‘label1’, ’label2’, ‘label3’]]
ser_obj[[pos1, pos2, pos3]]

# 根据条件取值(布尔索引)
# 对对象做运算，返回新对象，显示对象中每个元素的布尔值
(ser_obj1 > 10) & (ser_obj1 < 14)
# 对索引做布尔，返回符合条件的结果
ser_obj1[(ser_obj1 > 10) & (ser_obj1 < 14)]
ser_obj1[~(ser_obj1 == 12)]
```

#### DataFrame

```
# DataFrame对象本身也支持一些索引取值操作，但是通常情况下，会使用规范良好的高级索引方法。

# 指定index指定行索引，columns指定列索引
df_obj = pd.DataFrame(np.random.rand(3, 4), index= list("ABC"), columns=list("abcd"))

# 列索引取值
# 取某列(Series对象)
df_obj['column_label']
# 取某列的数据(ndarray对象)
df_obj['column_label'].values
# 取某列的某个数据
df_obj['column_label'].values[num]
df_obj['column_label']['raw_label']
# 取不连续的多列(不能取连续的列)
df_obj[['column_label1', 'column_label2']]
# 连续索引取单行
df_obj["raw_label1":"raw_label1"]
# 取连续的多行（不能取不连续的行）
df_obj["raw_label1":"raw_label2"]
# 按条件索引(布尔索引)
df_obj[df_obj["column_label] >= 2]
```

#### 高级索引

有3种：标签索引 loc、位置索引 iloc，混合索引ix
Series结构简单，一般不需要高级索引取值，主要用于DataFrame对象

- loc

```
# Series
# 1. 取连续多行
print(ser_obj1.loc['B' : 'C'])
# 2. 取多行
print(ser_obj1.loc[['A', 'C', 'D']])

# DataFrame
# 1.取单行
print(df_obj.loc["B"])
# 2.取单列
print(df_obj.loc[:, "d"])
# 3.取单行单列
print(df_obj.loc["B", "c"])
# 4.取连续多行
print(df_obj.loc["B":"D"])
# 5.取连续多列
print(df_obj.loc[:, "b":"d"])
# 6.取连续的多行多列
print(df_obj.loc["B":"D", "b":"d"])
# 7.取不连续的多行
print(df_obj.loc[["B", "D"]])
# 8.取不连续的多列
print(df_obj.loc[:, ["a", "c", "d"]])
# 9.取不连续的多行多列
print(df_obj.loc[["A","D"], ["a", "d"]])
# 10.取布尔值
# 根据某列的数据进行判断，返回为True的行
print(df_obj.loc[df_obj["a"] > -1])
# 根据某列的数据进行判断，返回为True的行，再取出指定的多列
print(df_obj.loc[df_obj["a"] > -1, ["b", "d"]])
```

- iloc

作用和loc一样，不过是基于索引编号来索引

```
# Series
print(ser_obj[1:3])
print(ser_obj.iloc[1:3])

# DataFrame
print(df_obj.iloc[0])
print(df_obj.iloc[0:2])
print(df_obj.iloc[0:2, 1:3])
print(df_obj.iloc[[0, 2], [0, 3]])
```

- ix

ix是以上二者的综合，既可以使用索引编号，又可以使用自定义索引，但是如果索引既有数字又有英文，容易导致定位的混乱。目前官方已不推荐使用

### 新增数据

```
# Series
# 新增行数据
ser_obj["F"] = 3.14

# DataFrame
# 新增列数据
df_obj["f"] = [10, 20, 30, 40]
df_obj["g"] = df_obj["c"] + df_obj["f"]
```

### 删除数据

```
# del 删除原数据，
del(df_obj['g'])

# drop() 
# 删除并返回原数据的副本，原数据不删除,默认axis=0
df_obj.drop(["f"], axis=1)
# 加inplace=True后是删除原数据
df_obj.drop(["C", "D"], inplace=True)
```

### 类型转换

```
# 索引对象支持类型转换：list,ndarray,Series
print(df_obj.index)
print(list(df_obj.index))
print(np.array(df_obj.index))
print(pd.Series(df_obj.index))

print(df_obj.columns)
print(list(df_obj.columns))
print(np.array(df_obj.columns))
print(pd.Series(df_obj.columns))
```

### 重命名

```
df_obj.rename(
    index = {"A" : "AA", "B" : "BB", "C" : "CC"}, 
    columns = {"a" : 'aa', "b" : "bb", "c" : "cc", "d" : "dd"}, 
    inplace=True
)
```

## 算数运算与数据对齐

- Pandas可以对不同索引的对象进行算术运算，如果存在不同的索引，结果的索引就是所有索引的并集。
- 如果索引相同，按索引对齐进行运算。如果没对齐的位置则补NaN；但是可以通过指定数据填充缺失值，再参与对齐运算。

### Series

```
#  Series 按行、索引对齐
ser_obj1 = pd.Series(range(10, 15), index=list("ABCDE"))
ser_obj2 = pd.Series(range(10, 15), index=list("CDEFG"))
# 索引对齐则进行算术运算；索引未对齐，则填充NaN值
ser_obj3 = ser_obj1.add(ser_obj2)
# 通过fill_value参数填充一个值参与对齐运算(注意不是填充到结果上)
ser_obj4 = ser_obj1.add(ser_obj2, fill_value=100)
```

### DataFrame

```
df_obj1 = pd.DataFrame(np.random.randint(-5, 10, (3, 4)), index=list("ABC"), columns=list("abcd"))
df_obj2 = pd.DataFrame(np.random.randint(-5, 10, (3, 4)), index=list("ABC"), columns=list("cdef"))
df_obj3 = df_obj1.add(df_obj2)
df_obj4 = df_obj1.add(df_obj2, fill_value=100)
```

## 数据清洗

- 数据的质量直接关乎最后数据分析出来的结果，如果数据有错误，在计算和统计后，结果也会有误。所以在进行数据分析前，我们必须对数据进行清洗。需要考虑数据是否需要修改、如何修改调整才能适用于之后的计算和分析等。
- 数据清洗也是一个迭代的过程，实际项目中可能需要不止一次地执行这些清洗操作。

### 处理缺失值

```
# 判断数据集是否有缺失值,返回bool类型的DataFrame
df_obj.isnull()
df_obj.notnull()

# 删除缺失值所在的行
df_obj.dropna()
# 删除缺失值所在的列
df_obj.dropna(axis=1)

# 填充缺失值
df_obj.fillna(value)
```

### 处理重复值

```
# 判断某列中是否有重复数据,返回bool类型的series/dataframe
df_obj.duplacited('column_label')

# 删除重复数据的行
df_obj.drop_duplicates('column_label')
```

### 进行替换值

```
# Series
# 1.单值替换：将所有的参数1的值替换为参数2的值
print(ser_obj.replace(1, 100))
# 2.多值替换：将所有的参数1的值替换为参数2的值
print(ser_obj.replace([0,2,4], 100))
# 3.不同值做不同替换
print(ser_obj.replace({1:100, 3:300}))

# DataFrame
print(df_obj3)
# 参数1是字典：表示指定列需要替换的指定值， 参数2表示替换后的值
print(df_obj3.replace({"b":7}, 700))
# 参数只有一个字典，表示不同的值做不同的替换
print(df_obj3.replace({6: 600, 7:700}))
# 参数1是列表，表示需要替换的值，替换为参数2的值
print(df_obj3.replace([6, 7], 1000))
# 替换指定值
print(df_obj3.replace(6, 600))
```

## 函数应用

### Numpy

```
print(np.sum(ser_obj))
print(np.abs(ser_obj))
print(np.sum(df_obj))
print(np.sum(df_obj, axis=1))
```

### apply

```
# 使用apply应用自定义函数到DataFrame的对象的每一行/每一列上,
# 返回值由自定义函数决定，若是计算类，返回DataFrame,若是统计类，返回Series

print(df.apply(lambda x : x.max()))
# 指定轴方向，axis=1，方向是行
print(df.apply(lambda x : x.max(), axis=1))
```

### applymap

```
# 通过applymap将函数应用到DataFrame的每个元素
# 返回DataFrame

print(df.applymap(lambda x : '%.2f' % x))
```

## 排序处理

```
# 按索引排序，排序默认使用升序排序，ascending=False 为降序排序
ser_obj.sort_index()
# 默认为行索变动排序，排序规则为升序,
# axis=0表示列变动，ascending=True表示降序
print(df_obj.sort_index())

# 按值排序
# by参数指定需要排序的列名(数字、字符串)
# 若有重名的列，不能参与排序
df_obj.sort_values(by='column_name', ascending=False))
```

## 层级索引与数据重构

### 索引对象

- Index，自定义索引
- RangeIndex，序列索引
- Int64Index，整数索引
- MultiIndex，层级索引

### 层级索引

```
# 选层
# 1.选取外层
ser_obj['outer_label']
# 2.选取指定外层的指定内层
print(ser_obj['b', '2'])
# 3.选取所有外层的指定内层
ser_obj[:, 'inner_label']

# 交换分层
# swaplevel表示交换指定的两个层级，参数为层级的下标，如果只有两层索引，默认就是内外层互相交换
# 参数0：表示最外层
# 参数1：表示第二外层
# 参数2：表示第三外层
# ...
ser_obj2 = ser_obj.swaplevel()

# 排序分层
# sortlevel和sort_index(level=)表示对指定层级进行排序，参数为层级的下标
# 参数0：表示最外层
# 参数1：表示第二外层
# 参数2：表示第三外层
# ...
print(ser_obj.swaplevel().sortlevel())
```

### 数据重构

```
# unstack()
# Series->DataFrame
# 默认1内层索引为列索引，0将最外层做为列索引
# df_obj = ser_obj3.unstack()
df_obj = ser_obj3.unstack(1)

# stack()
# DataFrame->Series
# 将列索引旋转为行索引，完成层级索引
df_obj.stack()

# DataFrame.T
# 将行和列索引互相调换
df_obj.T
```

## 统计计算和描述

```
# 常用的统计计算
# axis=0 按列统计，axis=1按行统计
# skipna默认为True，表示计算时排除NaN值
df_obj.sum(df_obj)
df_obj.mean(skipna=False)
df_obj.max(skipna=False)
df_obj.min(skipna=False)

# 常用的统计描述
df_obj.describe()
```

| 方法          | 说明                                         |
| ------------- | -------------------------------------------- |
| count         | 非NA值得数量                                 |
| describe      | 针对Series或各DataFrame列计算汇总统计        |
| min,max       | 计算最小值和最大值                           |
| argmin,argmax | 计算能够获取到最小值和最大值的索引位置(整数) |
| idxmin,idxmax | 计算能够获取到最小值和最大值的索引值         |
| quantile      | 计算样本的分位数(0到1)                       |
| sum           | 值的总和                                     |
| mean          | 值的平均数                                   |
| median        | 值的算数中位数                               |
| mad           | 根据平均值计算平均绝对离差                   |
| var           | 样本值的方差                                 |
| std           | 样本值的标准差                               |
| skew          | 样本值的偏度(三阶矩)                         |
| kurt          | 样本值的峰度(四阶矩)                         |
| cumsum        | 样本值的累计和                               |
| cummin/cummax | 样本值的累计最大值和累计最小值               |
| cumprod       | 样本值的累计积                               |
| diff          | 计算一阶差分(对事件序列很有用)               |
| pct_change    | 计算百分数变化                               |

## 数据连接

- pd.merge
- 根据单个或多个键将不同DataFrame的行连接起来
- 类似数据库的连接操作

```
import pandas as pd
import numpy as np

df_obj1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
                        'data1' : np.random.randint(0,10,7)})
df_obj2 = pd.DataFrame({'key': ['a', 'b', 'd'],
                        'data2' : np.random.randint(0,10,3)})
                        
# 默认将重叠列的列名作为“外键”进行连接
print(pd.merge(df_obj1, df_obj2))

# on显示指定“外键”,尤其对于多个同名列
print(pd.merge(df_obj1, df_obj2, on='key'))

# left_on，左侧数据的“外键”，right_on，右侧数据的“外键”
# 默认结果为内连接，可以通过how指定连接方式
# 默认how="inner"内连接，取交集数据
df_obj3 = df_obj1.rename(columns={'key':'key1'})
df_obj4 = df_obj2.rename(columns={'key':'key2'})
print(pd.merge(df_obj3, df_obj4, left_on='key1', right_on='key2'))
print(pd.merge(df_obj3, df_obj4, left_on="key1", right_on="key2", how="inner" ))
# how="outer"外连接，取并集结果
print(pd.merge(df_obj3, df_obj4, left_on="key1", right_on="key2", how="outer" ))
# how="left"左连接，保证左表的完整性
print(pd.merge(df_obj3, df_obj4, left_on="key1", right_on="key2", how="left" ))
# how="right"右连接，保证右表的完整性
print(pd.merge(df_obj3, df_obj4, left_on="key1", right_on="key2", how="right" ))

# 处理重复列名，suffixes添加前缀，默认为_x, _y
# suffixes参数接收一个元组/列表， 两个元素分别表示左表和游标的后缀
df_obj5 = pd.DataFrame({"key": list("ababcacb"),
    "data": np.random.randint(-5, 10, 8)})
df_obj6 = pd.DataFrame({"key": list("cbdcdb"),
    "data": np.random.randint(-5, 10, 6)})
print(pd.merge(df_obj5, df_obj6, on="key", suffixes=["_left", "_right"]))

# 使用行索引连接，left_index=True或right_index=True
df_obj7 = pd.DataFrame({"key" : list("ababcbac"),
    "data" : np.random.randint(-5, 10, 8)})

df_obj8 = pd.DataFrame({"data" : np.random.randint(-5, 10, 6)},
    index = list("cbdcdb"))
# left_on表示指定左表的key列做为外键，right_index=True表示使用右表行索引作为外键，进行关联
new_df = pd.merge(df_obj7, df_obj8, left_on="key", right_index=True, how="outer", suffixes=["_left", "_right"])
```

## 数据合并

- 沿轴方向将多个对象合并到一起

### np.concatenate

```
import numpy as np
import pandas as pd
arr1 = np.random.randint(0, 10, (3, 4))
arr2 = np.random.randint(0, 10, (3, 4))

# 默认axis=0,行变动
print(np.concatenate([arr1, arr2]))
# axis=1, 列变动
print(np.concatenate([arr1, arr2], axis=1))
```

### pd.concat

- 注意指定轴方向，默认axis=0
- join指定合并方式，默认为outer
- Series合并时查看行索引有无重复

```
# index 有重复的情况
ser_obj1 = pd.Series(np.random.randint(-5, 10, 4), index=range(4))
ser_obj2 = pd.Series(np.random.randint(-5, 10, 5), index=range(5))
ser_obj3 = pd.Series(np.random.randint(-5, 10, 6), index=range(6))
# 默认axis=0表示新增多行
print(pd.concat([ser_obj1, ser_obj2, ser_obj3]))
# axis=1表示新增多列
print(pd.concat([ser_obj1, ser_obj2, ser_obj3], axis=1))
# 默认join="outer"表示外连接，inner表示内连接
print(pd.concat([ser_obj1, ser_obj2, ser_obj3], axis=1, join="inner"))

#  index 没有重复的情况
ser_obj4 = pd.Series(np.random.randint(-5, 10, 4), index=range(4))
ser_obj5 = pd.Series(np.random.randint(-5, 10, 5), index=range(4, 9))
ser_obj6 = pd.Series(np.random.randint(-5, 10, 6), index=range(9, 15))
print(pd.concat([ser_obj4, ser_obj5, ser_obj6], axis=1))

# 多个DataFrame对象进行合并，注意索引是否一致(若索引不一致，合并没有任何意义)
df_obj1 = pd.DataFrame(np.random.randint(-5, 10, (3, 4)), index=list("ABC"), columns=list("abcd"))
df_obj2 = pd.DataFrame(np.random.randint(-5, 10, (4, 5)), index=list("ABCD"), columns=list("abcde"))
df_obj3 = pd.DataFrame(np.random.randint(-5, 10, (5, 6)), index=list("ABCDE"), columns=list("abcdef"))
# 默认axis=0表示共享列索引，新增多行
print(pd.concat([df_obj1, df_obj2, df_obj3], axis=0))
# axis=1表示共享行索引，新增多列
print(pd.concat([df_obj1, df_obj2, df_obj3], axis=1, join="inner")) 
```

## 数据分组

- 对数据集进行分组，然后对每组进行统计分析
- SQL能够对数据进行过滤，分组聚合
- pandas能利用groupby进行更加复杂的分组运算
- 分组运算过程：split->apply->combine
  1. 拆分：进行分组的根据
  2. 应用：每个分组运行的计算规则
  3. 合并：把每个分组的计算结果合并起来

### 分组操作

```
# 分组操作
# groupby()进行分组，GroupBy对象没有进行实际运算，只是包含分组的中间数据
# 按列名分组：obj.groupby(‘label’)
df_obj.groupby(df_obj['key1'])
df_obj['data1'].groupby(df_obj['key1'])

# 分组运算
# 对GroupBy对象进行分组运算/多重分组运算，如mean()
# 非数值数据不进行分组运算
obj.groupby(‘label’).mean()


# 按自定义的key列表分组
obj.groupby(self_def_key)，自定义的key可为列表，相当于新增一列，并按该列分组
self_def_key = [0, 1, 2, 3, 3, 4, 5, 7]
df_obj.groupby(self_def_key).size()
```

### 迭代操作

```
# GroupBy对象支持迭代操作
# 每次迭代返回一个元组 (group_name, group_data)，可用于分组数据的具体运算
# 单层分组，根据key1
for group_name, group_data in grouped1:
    print(group_name)
    print(group_data)
# 多层分组，根据key1 和 key2
for group_name, group_data in grouped2:
    print(group_name)
    print(group_data)    
```

### 转换

```
# GroupBy对象可以转换成列表或字典
# GroupBy对象转换list
print(list(grouped1))
# GroupBy对象转换dict
print(dict(list(grouped1)))
```



## 数据聚合

- 数组产生标量的过程，如mean()、count()等
- 常用于对分组后的数据进行计算

### 内置聚合函数

| 函数名     | 说明                        |
| ---------- | --------------------------- |
| count      | 分组中非NA值的数量          |
| sum        | 非NA值的和                  |
| mean       | 非NA值的平均值              |
| median     | 非NA值的算数中位数          |
| std/var    | 无偏(分母为n-1)标准差和方差 |
| min/max    | 非NA值的最小值和最大值      |
| prod       | 非NA值的积                  |
| first/last | 第一个和最后一个的非NA值    |

```
print(df_obj5.groupby('key1').sum())
print(df_obj5.groupby('key1').max())
print(df_obj5.groupby('key1').min())
print(df_obj5.groupby('key1').mean())
print(df_obj5.groupby('key1').size())
print(df_obj5.groupby('key1').count())
print(df_obj5.groupby('key1').describe())
```

### 自定义函数

grouped.agg(func)，func的参数为groupby索引对应的记录

```
func1 = lambda x: x.max() - x.min()
# agg()传入自定义函数 直接写函数名； 若是Pandas内置的函数，用字符串表示
print(df_obj.groupby(df_obj["key1"]).agg(func1))
print(df_obj.groupby(df_obj["key1"]).agg("sum"))

# 可以同时应用多个聚合函数(默认使用函数名作为列名)， 也可以再修改列名
df_obj.groupby(df_obj["key1"]).agg([("和", "sum"),("平均数", "mean"), ("最大值", "max"), ("最小值", "min"), ("大小差值", func1)])

# 可以对不同的列使用不同的聚合函数
dict_map = {
    "data1": ["sum", "mean"],
    "data2": ["max", "min", ("max-min", func1)]
}
print(df_obj.groupby(df_obj["key1"]).agg(dict_map))
```

## 分组聚合后的数据合并

```
import numpy as np
import pandas as pd

df_obj = pd.DataFrame(
    {
    "key1" : list("abbaabba"),
    "key2" : ["one", "two", "three", "two", "one", "three", "one", "two"],
    "data1" : np.random.randint(-5, 10, 8),
    "data2" : np.random.randint(-5, 10, 8),  
    }, index=list("ABCDEFGH")
)
```



### merge()

对分组聚合后的数据表和原表进行关联

```
# 对分组聚合后的结果进行多表关联，通过suffixes参数添加后缀区分同名列
key1_sum_df1 = df_obj.groupby(df_obj["key1"]).sum()
print(pd.merge(df_obj, key1_sum_df1, left_on="key1", right_index=True, suffixes=["_left", "_right"]))

# 使用add_prrefix()方法对分组聚合后的结果列名添加前缀，再进行多表关联
key1_sum_df2 = df_obj.groupby(df_obj["key1"]).sum().add_prefix("key1_sum_")
print(pd.merge(df_obj, key1_sum_df2,left_on="key1",right_index=True ))
```

### transform()

接收聚合函数作为参数，运算结果默认和原表形状一致，直接参与concat合并

```
key1_tf_df = df_obj[["data1", "data2"]].groupby(df_obj["key1"]).transform("sum").add_prefix("key1_sum_")
print(pd.concat([df_obj, key1_tf_df], axis=1))
```

### groupby.app(func)

GroupBy对象可以通过apply方法，将自定义函数在各各个分组上分别调用，最后的结果自动通过pd.concat合并到一起。

```
import numpy as np
import pandas as pd

filename = "./starcraft.csv"
df_obj = pd.read_csv(filename, usecols=["LeagueIndex", "Age", "HoursPerWeek", "TotalHours", "APM"])
print(df_obj.head())

# 统计各个段位里手速最快的前3个人
def top_n(df, column="APM", n=3, sort_type=False):
    return df.sort_values(by=column, ascending=sort_type)[:n]
    
# 通过apply调用自定义函数，参数是分组后的每组数据
# 通过自定义函数的计算后，并返回结果，最后自动合并为一个DataFrame
print(df_obj.groupby(df_obj["LeagueIndex"]).apply(top_n))
# apply()

# 参数1：分组后的每组数据，后面的参数可传给自定义函数接收
print(df_obj.groupby(df_obj["LeagueIndex"]).apply(top_n, "Age", 5, True))

# 分组以及默认为行索引，可以通过group_keys=False禁用分组一句作为行索引
print(df_obj.groupby(df_obj["LeagueIndex"], group_keys=False).apply(top_n, "Age", 5, True))

```

