# 17 Pandas groupby分组操作

在数据分析中，经常会遇到这样的情况：根据某一列（或多列）标签把数据划分为不同的组别，然后再对其进行数据分析。比如，某网站对注册用户的性别或者年龄等进行分组，从而研究出网站用户的画像（特点）。在 Pandas 中，要完成数据的分组操作，需要使用 groupby() 函数，它和 SQL 的`GROUP BY`操作非常相似。 

在划分出来的组（group）上应用一些统计函数，从而达到数据分析的目的，比如对分组数据进行聚合、转换，或者过滤。这个过程主要包含以下三步：

- 拆分（Spliting）：表示对数据进行分组；
- 应用（Applying）：对分组数据应用聚合函数，进行相应计算；
- 合并（Combining）：最后汇总计算结果。

下面对 groupby() 函数的应用过程进行具体的讲解。

## 创建DataFrame对象

首先我们创建一个 DataFrame 对象，下面数据描述了某班学生，计算机选修课的考试成绩：

```python
import pandas as pd 
import numpy as np 
data = {'Name': ['John', 'Helen', 'Sona', 'Ella', 'Jacky', 'Eric'], 
   'score': [82, 98, 91, 87, 82, 91], 
   'option_course': ['C#','Python','Java','C', 'C#', 'C++']} 
df = pd.DataFrame(data)
print(df)  
```

输出结果：

```
    Name  score option_course
0   John     82            C#
1  Helen     98        Python
2   Sona     91          Java
3   Ella     87             C
4  Jacky     82            C#
5   Eric     91           C++
```

## 创建groupby分组对象

使用 groupby() 可以沿着任意轴分组。您可以把分组时指定的键（key）作为每组的组名，方法如下所示：

- df.groupby("key")
- df.groupby("key",axis=1)
- df.groupby(["key1","key2"])

通过上述方法对 DataFrame 对象进行分组操作：

```python
#生成分组groupby对象
print(df.groupby('score'))
#等价写法
print(df.groupby(['score']))
```

输出结果：

```
<pandas.core.groupby.generic.DataFrameGroupBy object at 0x00000249A8E5F850>
```

## 查看分组结果

#### 1) groups查看分组结果

通过调用`groups`属性查看分组结果：

```python
#查看分组
print(df.groupby('score').groups)
```

输出结果：

```
{82: [0, 4], 87: [3], 91: [2, 5], 98: [1]}
```

#### 2) 多个列标签分组

当然也可以指定多个列标签进行分组，示例如下：

```python
#查看分组
print(df.groupby(['score','option_course']).groups)
```

输出结果：

```
{(82, 'C#'): [0, 4], (87, 'C'): [3], (91, 'C++'): [5], (91, 'Java'): [2], (98, 'Python'): [1]}
```

通过 get_group() 方法可以选择组内的具体数据项：

```python
#根据score来分组
grouped=df.groupby('score')
#根据对应组的数据值，选择一个组
print(grouped.get_group(91))
```

输出结果：

```
   Name  score option_course
2  Sona     91          Java
5  Eric     91           C++
```

## 遍历分组数据

通过以下方法来遍历分组数据，示例如下：

```python
#查看分组
grouped=df.groupby('score')
for label, option_course in grouped:
#其中key代表分组后字典的键，也就是score
    print(label)
#字典对应的值选修的科目
    print(option_course)
```

输出结果：

```
82
    Name  score option_course
0   John     82            C#
4  Jacky     82            C#
87
   Name  score option_course
3  Ella     87             C
91
   Name  score option_course
2  Sona     91          Java
5  Eric     91           C++
98
    Name  score option_course
1  Helen     98        Python
```

如上所示， groupby 对象的组名称与 score 中的的元素值一一对应。

## 查看每组的统计数据

```python
grouped=df.groupby('option_course')
#查看所有列的统计信息
print(grouped.describe())
```

输出结果：

```
              score                                         
              count  mean  std   min   25%   50%   75%   max
option_course                                               
C               1.0  87.0  NaN  87.0  87.0  87.0  87.0  87.0
C#              2.0  82.0  0.0  82.0  82.0  82.0  82.0  82.0
C++             1.0  91.0  NaN  91.0  91.0  91.0  91.0  91.0
Java            1.0  91.0  NaN  91.0  91.0  91.0  91.0  91.0
Python          1.0  98.0  NaN  98.0  98.0  98.0  98.0  98.0
```

查看纵向视图：

```python
grouped=df.groupby('option_course')
#查看所有列的统计信息纵向视图
print(grouped.describe().unstack())
```

输出结果：

```
             option_course
score  count  C                 1.0
              C#                2.0
              C++               1.0
              Java              1.0
              Python            1.0
       mean   C                87.0
              C#               82.0
              C++              91.0
              Java             91.0
              Python           98.0
       std    C                 NaN
              C#                0.0
              C++               NaN
              Java              NaN
              Python            NaN
       min    C                87.0
              C#               82.0
              C++              91.0
              Java             91.0
              Python           98.0
       25%    C                87.0
              C#               82.0
              C++              91.0
              Java             91.0
              Python           98.0
       50%    C                87.0
              C#               82.0
              C++              91.0
              Java             91.0
              Python           98.0
       75%    C                87.0
              C#               82.0
              C++              91.0
              Java             91.0
              Python           98.0
       max    C                87.0
              C#               82.0
              C++              91.0
              Java             91.0
              Python           98.0
dtype: float64
```

## 应用聚合函数

数据表中的列按值是否连续，可以分为连续值列、离散值列。对于离散值列，可以统计其不重复值的个数。对于连续值列，统计不重复值一般没有意义。统计结果是一个Series对象：

```python
grouped=df.groupby('option_course')
#统计离散值列个数
print(grouped.value_counts())
```

输出结果：

```
option_course  Name   score
C              Ella   87       1
C#             Jacky  82       1
               John   82       1
C++            Eric   91       1
Java           Sona   91       1
Python         Helen  98       1
dtype: int64
```

组内数值列和，只有数值列能求和，非数值列不可以：

```python
grouped=df.groupby('option_course')
#求均值
print(grouped['score'].mean())
```

输出结果：

```
option_course
C         87.0
C#        82.0
C++       91.0
Java      91.0
Python    98.0
Name: score, dtype: float64
```

| 函数名称       | 说明                 |
| -------------- | -------------------- |
| count()        | 组内成员数。         |
| mean()         | 组内数值列均值。     |
| median()       | 组内数值列中位数。   |
| sum()          | 组内数值列和。       |
| value_counts() | 组内离散列计数。     |
| std()          | 组内数值列标准差。   |
| min()          | 组内数值列最小值。   |
| max()          | 组内数值列最大值。   |
| var()          | 组内数值列方差。     |
| quantile()     | 组内数值列二分位数。 |
| cumsum()       | 组内数值列累计和。   |

每组内，可以指定只求某一列的统计指标，包括平均数，方差等：

```python
grouped=df.groupby('option_course')
print(grouped['score'].apply(np.mean))
```

输出结果：

```
option_course
C         87.0
C#        82.0
C++       91.0
Java      91.0
Python    98.0
Name: score, dtype: float64
```

当您在创建 groupby 对象时，通过 agg() 函数可以对分组对象应用多个聚合函数，示例如下：

```python
grouped=df.groupby('option_course')
print(grouped['score'].agg([np.mean,np.std]))
```

输出结果：

```
               mean  std
option_course           
C              87.0  NaN
C#             82.0  0.0
C++            91.0  NaN
Java           91.0  NaN
Python         98.0  NaN
```

组内不同列用不同函数：

```python
grouped=df.groupby('option_course')
print(grouped.agg({'score':np.mean,'Name':np.size}))
```

输出结果：

```
               score  Name
option_course             
C               87.0     1
C#              82.0     2
C++             91.0     1
Java            91.0     1
Python          98.0     1
```

## 组的转换操作

在组的行或列上可以执行转换操作，最终会返回一个与组大小相同的索引对象。示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame({'Type':['Fruit','Fruit','Fruit','Vegetable','Vegetable','Meet','Meet'],
                'Origin':['UK','China','Spain','China','Spain','Germany','France'],
                'Commodity':['Apple','Orange','Melon','Celery','Cabbage','Chicken','Beef'],
                'Quantity':[3,5,5,3,2,15,9],
                'Price':[2,5,12,3,4,18,20]})
#分组求均值，水果、蔬菜、肉类
#对可执行计算的数值列求均值
grouped=df.groupby('Type')
print(grouped[['Quantity', 'Price']].transform(np.mean))
#transform()计算值和均值的差
grouped=df.groupby('Type')
demean = lambda arr:arr-arr.mean()
print(grouped[['Quantity', 'Price']].transform(demean))
#transform()计算最大值和最小值差
grouped=df.groupby('Type')
maxdiff = lambda x: x.max() - x.min()
print(grouped[['Quantity', 'Price']].transform(maxdiff))
#自定义函数返回分组的前n行数据
def get_rows(df,n): 
     #从1到n行的所有列
    return df.iloc[:n,:]
#分组后的组名作为行索引
grouped=df.groupby('Type')
print(grouped.apply(get_rows,n=2))
```

输出结果：

```
        Type   Origin Commodity  Quantity  Price
0      Fruit       UK     Apple         3      2
1      Fruit    China    Orange         5      5
2      Fruit    Spain     Melon         5     12
3  Vegetable    China    Celery         3      3
4  Vegetable    Spain   Cabbage         2      4
5       Meet  Germany   Chicken        15     18
6       Meet   France      Beef         9     20

    Quantity      Price
0   4.333333   6.333333
1   4.333333   6.333333
2   4.333333   6.333333
3   2.500000   3.500000
4   2.500000   3.500000
5  12.000000  19.000000
6  12.000000  19.000000

   Quantity     Price
0 -1.333333 -4.333333
1  0.666667 -1.333333
2  0.666667  5.666667
3  0.500000 -0.500000
4 -0.500000  0.500000
5  3.000000 -1.000000
6 -3.000000  1.000000

   Quantity  Price
0         2     10
1         2     10
2         2     10
3         1      1
4         1      1
5         6      2
6         6      2

                  Type   Origin Commodity  Quantity  Price
Type                                                      
Fruit     0      Fruit       UK     Apple         3      2
          1      Fruit    China    Orange         5      5
Meet      5       Meet  Germany   Chicken        15     18
          6       Meet   France      Beef         9     20
Vegetable 3  Vegetable    China    Celery         3      3
          4  Vegetable    Spain   Cabbage         2      4
```

## 组的数据过滤操作

通过 filter() 函数可以实现数据的筛选，该函数根据定义的条件过滤数据并返回一个新的数据集。

下面，筛选出参加比赛超过两次的球队（包含两次）：

```python
import pandas as pd
import numpy as np
data = {'Team': ['Riders', 'Riders', 'Devils', 'Devils', 'Kings',
   'Kings', 'Kings', 'Kings', 'Riders', 'Royals', 'Royals', 'Riders'],
   'Rank': [1, 2, 2, 4, 3, 1, 3, 1, 2, 4, 1, 2],
   'Year': [2022,2021,2022,2021,2022,2019,2021,2020,2019,2022,2021,2020],
   'Points':[874,789,863,663,741,802,756,788,694,701,812,698]}
df = pd.DataFrame(data)
#定义lambda函数来筛选数据
grouped=df.groupby(['Team', 'Year'])
print(df)
print (grouped.filter(lambda x: x['Points'].mean() >= 770))
```

输出结果：

```
      Team  Rank  Year  Points
0   Riders     1  2022     874
1   Riders     2  2021     789
2   Devils     2  2022     863
3   Devils     4  2021     663
4    Kings     3  2022     741
5    Kings     1  2019     802
6    Kings     3  2021     756
7    Kings     1  2020     788
8   Riders     2  2019     694
9   Royals     4  2022     701
10  Royals     1  2021     812
11  Riders     2  2020     698

      Team  Rank  Year  Points
0   Riders     1  2022     874
1   Riders     2  2021     789
2   Devils     2  2022     863
5    Kings     1  2019     802
7    Kings     1  2020     788
10  Royals     1  2021     812
```