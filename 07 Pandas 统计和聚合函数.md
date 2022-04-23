# 07 Pandas统计和聚合函数

## 描述性统计函数

描述统计学（descriptive statistics）是一门统计学领域的学科，主要研究如何取得反映客观现象的数据，并以图表形式对所搜集的数据进行处理和显示，最终对数据的规律、特征做出综合性的描述分析。Pandas 库正是对描述统计学知识完美应用的体现，可以说如果没有“描述统计学”作为理论基奠，那么 Pandas 是否存在犹未可知。下列表格对 Pandas 常用的统计学函数做了简单的总结：

| 函数名称  | 描述说明                                                     |
| --------- | ------------------------------------------------------------ |
| count()   | 统计某个非空值的数量。                                       |
| sum()     | 求和                                                         |
| mean()    | 求均值                                                       |
| median()  | 求中位数                                                     |
| mode()    | 求众数                                                       |
| std()     | 求标准差                                                     |
| min()     | 求最小值                                                     |
| max()     | 求最大值                                                     |
| abs()     | 求绝对值                                                     |
| prod()    | 求所有数值的乘积。                                           |
| cumsum()  | 计算累计和，axis=0，按照行累加；axis=1，按照列累加。         |
| cumprod() | 计算累计积，axis=0，按照行累积；axis=1，按照列累积。         |
| corr()    | 计算数列或变量之间的相关系数，取值-1到1，值越大表示关联性越强。 |


从描述统计学角度出发，我们可以对 DataFrame 结构执行聚合计算等其他操作，比如 sum() 求和、mean()求均值等方法。

在 DataFrame 中，使用聚合类方法时需要指定轴(axis)参数。下面介绍两种传参方式：

- 对行操作，默认使用 axis=0 或者使用 "index"；
- 对列操作，默认使用 axis=1 或者使用 "columns"。



![axis轴示意图](http://c.biancheng.net/uploads/allimg/210901/1556103322-0.gif)
图1：axis轴示意图

从图 1 可以看出，axis=0 表示按垂直方向进行计算，而 axis=1 则表示按水平方向。

数值型数据的描述性统计主要包括了计算数值型数据的完整情况、最小值、均值、中位 数、最大值、四分位数、极差、标准差、方差、协方差等。下面让我们创建一个 DataFrame，使用它对本节的内容进行演示：

```python
import pandas as pd
import numpy as np
#创建字典型series结构
d = {'Name':pd.Series(['Tom','James','Ricky','Vin','Steve','Minsu','Jack',
   'Lee','David','Gasper','Betina','Andres']),
   'Age':pd.Series([25,26,25,23,30,29,23,34,40,30,51,46]),
   'Rating':pd.Series([4.23,3.24,3.98,2.56,3.20,4.6,3.8,3.78,2.98,4.80,4.10,3.65])}
df = pd.DataFrame(d)
print(df)
```

输出结果：

```
      Name  Age  Rating
0      Tom   25    4.23
1    James   26    3.24
2    Ricky   25    3.98
3      Vin   23    2.56
4    Steve   30    3.20
5    Minsu   29    4.60
6     Jack   23    3.80
7      Lee   34    3.78
8    David   40    2.98
9   Gasper   30    4.80
10  Betina   51    4.10
11  Andres   46    3.65
```

使用各个描述性统计函数进行计算：

```python
#默认axis=0或者使用sum("index")
print(df['Rating'].sum())
#也可使用sum("columns")或sum(1)
print(df.sum(axis=1))
#使用mean求均值
print(df['Rating'].mean())
#使用std求标准差
print(df['Rating'].std())
#对字符串数据使用sum
print(df['Name'].sum())
#对字符串数据使用cumsum
print(df['Name'].cumsum())
```

输出结果：

```
44.919999999999995

0     29.23
1     29.24
2     28.98
3     25.56
4     33.20
5     33.60
6     26.80
7     37.78
8     42.98
9     34.80
10    55.10
11    49.65
dtype: float64

3.7433333333333336

0.6616279280316959

TomJamesRickyVinSteveMinsuJackLeeDavidGasperBetinaAndres

0                                                   Tom
1                                              TomJames
2                                         TomJamesRicky
3                                      TomJamesRickyVin
4                                 TomJamesRickyVinSteve
5                            TomJamesRickyVinSteveMinsu
6                        TomJamesRickyVinSteveMinsuJack
7                     TomJamesRickyVinSteveMinsuJackLee
8                TomJamesRickyVinSteveMinsuJackLeeDavid
9          TomJamesRickyVinSteveMinsuJackLeeDavidGasper
10    TomJamesRickyVinSteveMinsuJackLeeDavidGasperBe...
11    TomJamesRickyVinSteveMinsuJackLeeDavidGasperBe...
Name: Name, dtype: object
```

**注意**：sum() 和 cumsum() 函数可以同时处理数字和字符串数据。虽然字符聚合通常不被使用，但使用这两个函数并不会抛出异常；而对于 abs()、cumprod() 函数则会抛出异常，因为它们无法操作字符串数据。但是应该养成良好的习惯，在进行描述性统计之前应该选择有效列， 在未来的版本中，使用无效的数据类型将引发TypeError。

## 数据汇总描述 (describe)

describe() 函数显示与 DataFrame 数据列相关的统计信息摘要。示例如下：

```python
print(df.describe())
```

输出结果：

```
             Age     Rating
count  12.000000  12.000000
mean   31.833333   3.743333
std     9.232682   0.661628
min    23.000000   2.560000
25%    25.000000   3.230000
50%    29.500000   3.790000
75%    35.500000   4.132500
max    51.000000   4.800000
```

describe() 函数输出了平均值、std 和 IQR 值(四分位距)等一系列统计信息。通过 describe() 提供的`include`能够筛选字符列或者数字列的摘要信息。

include 相关参数值说明如下：

- object： 表示对字符列进行统计信息描述；
- number：表示对数字列进行统计信息描述；
- all：汇总所有列的统计信息。


下面看一组示例，如下所示：

```python
print(df.describe(include=["object"]))
```

输出结果：

```
       Name
count    12
unique   12
top     Tom
freq      1
```

最后使用`all`参数，看一下输出结果，如下所示：

```python
print(df.describe(include="all"))
```

输出结果：

```    
       Name        Age     Rating
count    12  12.000000  12.000000
unique   12        NaN        NaN
top     Tom        NaN        NaN
freq      1        NaN        NaN
mean    NaN  31.833333   3.743333
std     NaN   9.232682   0.661628
min     NaN  23.000000   2.560000
25%     NaN  25.000000   3.230000
50%     NaN  29.500000   3.790000
75%     NaN  35.500000   4.132500
max     NaN  51.000000   4.800000
```

我们知道，窗口函数可以与聚合函数一起使用，聚合函数指的是对一组数据求总和、最大值、最小值以及平均值的操作，本节重点讲解聚合函数的应用。

## 聚合函数 (aggregate)

使用aggregate()在指定轴上使用一项或多项操作进行聚合，在NumPy库中一些常用的统计学函数也可用于对数据框进行描述性统计。它们包括：

| 函数   | 含义   | 函数      | 含义   | 数      | 含义   |
| ------ | ------ | --------- | ------ | ------- | ------ |
| np.min | 最小值 | np.max    | 最大值 | np.mean | 均值   |
| np.ptp | 极差   | np.median | 中位数 | np.std  | 标准差 |
| np.var | 方差   | np.cov    | 协方差 |         |        |

首先让我们创建一个 DataFrame 对象，然后对聚合函数进行应用。

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5, 4),index = pd.date_range('12/14/2022', periods=5),columns = ['A', 'B', 'C', 'D'])
print (df)
#窗口大小为3，min_periods 最小观测值为1
r = df.rolling(window=3,min_periods=1)
print(r) 
```

输出结果：

```
                   A         B         C         D
2022-12-14  0.365679  1.605576  0.094622  1.367596
2022-12-15 -0.003054 -0.311650  1.476279 -0.252426
2022-12-16  0.702722  0.444366  1.214280 -0.890047
2022-12-17 -0.265698 -1.174423  1.695452 -0.079901
2022-12-18 -0.266886  0.498338 -1.803101 -1.296304

Rolling [window=3,min_periods=1,center=False,axis=0,method=single]
```

### 1) 对整体聚合

您可以把一个聚合函数传递给 DataFrame，示例如下：

```python
#使用 aggregate()聚合操作
print(r.aggregate(np.sum))
```

输出结果：

```
                   A         B         C         D
2022-12-14  0.365679  1.605576  0.094622  1.367596
2022-12-15  0.362625  1.293926  1.570901  1.115170
2022-12-16  1.065348  1.738291  2.785181  0.225123
2022-12-17  0.433971 -1.041707  4.386011 -1.222374
2022-12-18  0.170139 -0.231719  1.106631 -2.266252
```

### 2) 对任意某一列聚合

```python
#对 A 列聚合
print(r['A'].aggregate(np.sum))
```

输出结果：

```
2022-12-14    0.365679
2022-12-15    0.362625
2022-12-16    1.065348
2022-12-17    0.433971
2022-12-18    0.170139
Freq: D, Name: A, dtype: float64
```

### 3) 对多列数据聚合

```python
#对 A/B 两列聚合
print(r['A','B'].aggregate(np.sum))
```

输出结果：

```
                   A         B
2022-12-14  0.365679  1.605576
2022-12-15  0.362625  1.293926
2022-12-16  1.065348  1.738291
2022-12-17  0.433971 -1.041707
2022-12-18  0.170139 -0.231719
```

### 4) 对单列应用多个函数

```python
#对 A 列聚合
print(r['A'].aggregate([np.sum,np.mean]))
```

输出结果：

```
                 sum      mean
2022-12-14  0.365679  0.365679
2022-12-15  0.362625  0.181313
2022-12-16  1.065348  0.355116
2022-12-17  0.433971  0.144657
2022-12-18  0.170139  0.056713
```

### 5) 对不同列应用多个函数

```python
#对 A/B 两列聚合
print(r['A','B'].aggregate([np.sum,np.mean]))
```

输出结果：

```
                   A                   B          
                 sum      mean       sum      mean
2022-12-14  0.365679  0.365679  1.605576  1.605576
2022-12-15  0.362625  0.181313  1.293926  0.646963
2022-12-16  1.065348  0.355116  1.738291  0.579430
2022-12-17  0.433971  0.144657 -1.041707 -0.347236
2022-12-18  0.170139  0.056713 -0.231719 -0.077240
```

### 6) 对不同列应用不同函数

```python
print(r.aggregate({'A': np.sum,'B': np.mean}))
```

输出结果：

```
                   A         B
2022-12-14  0.365679  1.605576
2022-12-15  0.362625  0.646963
2022-12-16  1.065348  0.579430
2022-12-17  0.433971 -0.347236
2022-12-18  0.170139 -0.077240
```

Pandas 的本质是统计学原理在计算机领域的一种应用实现，通过编程的方式达到分析、描述数据的目的。而统计函数则是统计学中用于计算和分析数据的一种工具。在数据分析的过程中，使用统计函数有助于我们理解和分析数据。本节将学习几个常见的统计函数，比如百分比函数、协方差函数、相关系数等。

## 其他统计函数

### 1) 百分比变化(pct_change)

Series 和 DatFrames 都可以使用 pct_change() 函数。该函数**将每个元素与其前一个元素进行比较**，并计算前后数值的百分比变化。示例如下：

```python
import pandas as pd
import numpy as np
#Series结构
s = pd.Series([1,2,3,4,5,4])
print (s.pct_change())
#DataFrame
df = pd.DataFrame(np.random.randn(5, 2))
print(df.pct_change())
```

输出结果：

```
0         NaN
1    1.000000
2    0.500000
3    0.333333
4    0.250000
5   -0.200000
dtype: float64
           0          1
0        NaN        NaN
1  74.779242   0.624260
2  -0.353652  -1.104352
3  -2.422813 -13.994103
4  -3.828316  -1.853092
```

默认情况下，pct_change() 对列进行操作，如果想要操作行，则需要传递参数 axis=1 参数。示例如下：

```python
import pandas as pd
import numpy as np
#DataFrame
df = pd.DataFrame(np.random.randn(3, 2))
print(df.pct_change(axis=1))
```

输出结果：

```
    0         1
0 NaN  3.035670
1 NaN -0.318259
2 NaN  0.227580
```

### 2) 离散差(diff)

Series 和 DatFrames 都可以使用 diff() 函数。该函数**将每个元素与其前一个元素进行比较**，并计算前后数值的差值。示例如下：

```python
import pandas as pd
import numpy as np
#Series结构
s = pd.Series([1,2,3,4,5,4])
print (s.diff())
#DataFrame
df = pd.DataFrame(np.random.randn(5, 2))
print(df.diff())
```

输出结果：

```
0    NaN
1    1.0
2    1.0
3    1.0
4    1.0
5   -1.0
dtype: float64
          0         1
0       NaN       NaN
1  3.550505  2.203291
2 -0.717203 -2.853614
3  1.099947  2.554052
4 -2.408368 -0.404075
```

### 3) 协方差(cov)

Series 对象提供了一个`cov`方法用来计算 Series 对象之间的协方差。同时，该方法也会将缺失值(NAN )自动排除。

示例如下：

```python
import pandas as pd
import numpy as np
s1 = pd.Series(np.random.randn(10))
s2 = pd.Series(np.random.randn(10))
print (s1.cov(s2))
```

输出结果：

```
0.20789380904226645
```

当应用于 DataFrame 时，协方差（cov）将计算所有列之间的协方差。 

```python
import pandas as pd
import numpy as np
frame = pd.DataFrame(np.random.randn(10, 5), columns=['a', 'b', 'c', 'd', 'e'])
#计算a与b之间的协方差值
print (frame['a'].cov(frame['b']))
#计算所有数列的协方差值
print (frame.cov())
```

输出结果：

```
-0.37822395480394827

          a         b         c         d         e
a  1.643529 -0.378224  0.181642  0.049969 -0.113700
b -0.378224  1.561760 -0.054868  0.144664 -0.231134
c  0.181642 -0.054868  0.628367 -0.125703  0.324442
d  0.049969  0.144664 -0.125703  0.480301 -0.388879
e -0.113700 -0.231134  0.324442 -0.388879  0.848377
```

### 4) 相关系数(corr)

相关系数显示任意两个 Series 之间的线性关系。Pandas 提供了计算相关性的三种方法，分别是 pearson(default)、spearman() 和 kendall()。

* pearson：皮尔森相关系数，相关系数来衡量两个数据集合是否在一条线上面，即针对线性数据的相关系数计算，针对非线性                                           数据便会有误差。 

* kendall：Kendall Tau相关系数，用于反映分类变量相关性的指标，即针对无序序列的相关系数，非正态分布的数据 

* spearman：spearman秩相关，非线性的，非正态分析的数据的相关系数

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(10, 5), columns=['a', 'b', 'c', 'd', 'e'])
print (df['b'].corr(frame['c']))
print (df.corr())
#print (df.corr(method='pearson', min_periods=1))
```

输出结果：

```
0.5540831507407936

          a         b         c         d         e
a  1.000000 -0.500903 -0.058497 -0.767226  0.218416
b -0.500903  1.000000 -0.091239  0.805388 -0.020172
c -0.058497 -0.091239  1.000000  0.115905  0.083969
d -0.767226  0.805388  0.115905  1.000000  0.015028
e  0.218416 -0.020172  0.083969  0.015028  1.000000
```

注意：如果 DataFrame 存在非数值(NAN），该方法会自动将其删除。

### 5) 排名(rank)

rank() 按照某种规则（升序或者降序）对序列中的元素值排名，该函数的返回值的也是一个序列，包含了原序列中每个元素值的名次。如果序列中包含两个相同的的元素值，那么会为其**分配两者的平均排名**。示例如下：

```python
import pandas as pd
import numpy as np
#返回5个随机值，然后使用rank对其排名
s = pd.Series(np.random.randn(5), index=list('abcde'))
s['d'] = s['b']
print(s) 
#a/b排名分别为2和3，其平均排名为2.5
print(s.rank())
```

输出结果：

```
a   -0.689585
b   -0.545871
c    0.148264
d   -0.545871
e   -0.205043
dtype: float64

排名后输出:
a    1.0
b    2.5
c    5.0
d    2.5
e    4.0
dtype: float64
```

#### 1) method参数

rank() 提供了 method 参数，可以针对相同数据，进行不同方式的排名。如下所示：

- average：默认值，如果数据相同则分配平均排名；
- min：给相同数据分配最低排名；
- max：给相同数据分配最大排名；
- first：对于相同数据，根据出现在数组中的顺序进行排名。

#### 2) aisx&ascening

rank() 有一个`ascening`参数， 默认为 True 代表升序；如果为 False，则表示降序排名（将较大的数值分配给较小的排名）。

rank() 默认按行方向排名（axis=0），也可以更改为 axis =1，按列排名。示例如下：

```python
import pandas as pd
import numpy as np
a = pd.DataFrame(np.arange(12).reshape(3,4),columns = list("abdc"))
a =a.sort_index(axis=1,ascending=False)
a.iloc[[1,1],[1,2]] = 6
#按行排名，将相同数值设置为所在行数值的最大排名
print(a.rank(axis=1,method="max"))
```

输出结果：

```
     d    c    b    a
0  3.0  4.0  2.0  1.0
1  4.0  4.0  4.0  1.0
2  3.0  4.0  2.0  1.0
```

与 method="min"进行对比，如下所示：

```python
import pandas as pd
import numpy as np
a = pd.DataFrame(np.arange(12).reshape(3,4),columns = list("abdc"))
a =a.sort_index(axis=1,ascending=False)
a.iloc[[1,1],[1,2]] = 6
#按行排名，将相同数值设置为所在行数值的最小排名
print(a.rank(axis=1,method="min"))
```

输出结果：

```
     d    c    b    a
0  3.0  4.0  2.0  1.0
1  2.0  2.0  2.0  1.0
2  3.0  4.0  2.0   1.0
```

0