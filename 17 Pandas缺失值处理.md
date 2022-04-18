# 16 Pandas缺失值处理

在一些数据分析业务中，数据缺失是我们经常遇见的问题，缺失值会导致数据质量的下降，从而影响模型预测的准确性，这对于机器学习和数据挖掘影响尤为严重。因此妥善的处理缺失值能够使模型预测更为准确和有效。

## 为什么会存在缺失值？

前面章节的示例中，我们遇到过很多 NaN 值，关于缺失值您可能会有很多疑问，数据为什么会丢失数据呢，又是从什么时候丢失的呢？通过下面场景，您会得到答案。

其实在很多时候，人们往往不愿意过多透露自己的信息。假如您正在对用户的产品体验做调查，在这个过程中您会发现，一些用户很乐意分享自己使用产品的体验，但他是不愿意透露自己的姓名和联系方式；还有一些用户愿意分享他们使用产品的全部经过，包括自己的姓名和联系方式。因此，总有一些数据会因为某些不可抗力的因素丢失，这种情况在现实生活中会经常遇到。

## 什么是稀疏数据？

稀疏数据，指的是在数据库或者数据集中存在**大量缺失数据或者空值**，我们把这样的数据集称为稀疏数据集。稀疏数据不是无效数据，只不过是信息不全而已，只要通过适当的方法就可以“变废为宝”。

稀疏数据的来源与产生原因有很多种，大致归为以下几种：

- 由于调查不当产生的稀疏数据；
- 由于天然限制产生的稀疏数据；
- 文本挖掘中产生的稀疏数据。

## 缺失值处理

那么 Pandas 是如何处理缺失值的呢，下面让我们一起看一下。

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5, 3), index=['a', 'c', 'e', 'f','h'])
df = df.reindex(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'])
print(df)
```

输出结果：

```
          0         1         2
a  2.956663 -1.568245 -0.078136
b       NaN       NaN       NaN
c -1.547343 -0.643299  1.660468
d       NaN       NaN       NaN
e -0.097855  0.885068 -0.064441
f -0.503905 -0.423733  0.606880
g       NaN       NaN       NaN
h -0.729871  1.731587 -0.545545
```

上述示例，通过使用 reindex（重构索引），我们创建了一个存在缺少值的 DataFrame 对象。

## 检查缺失值

为了使检测缺失值变得更容易，Pandas 提供了 isnull() 和 notnull() 两个函数，它们同时适用于 Series 和 DataFrame 对象。

```python
print(df['0'].isnull())
```

输出结果：

```
a    False
b     True
c    False
d     True
e    False
f    False
g     True
h    False
Name: 0, dtype: bool
```

notnull() 函数，使用示例：

```python
print(df[0].notnull())
```

输出结果：

```
a     True
b    False
c     True
d    False
e     True
f     True
g    False
h     True
Name: 0, dtype: bool
```

## 缺失数据计算

计算缺失数据时，需要注意两点：首先数据求和时，将 NA 值视为 0 ，其次，如果要计算的数据为 NA，那么结果就是 NA。示例如下：

```python
print (df[0].sum())
```

输出结果：

```
0.07768978903781432
```

## 清理并填充缺失值

Pandas 提供了多种方法来清除缺失值。fillna() 函数可以实现用非空数据“填充”NaN 值。

#### 1) 用标量值替换NaN值

下列程序将 NaN 值 替换为了 0，如下所示：

```python
df = pd.DataFrame(np.random.randn(3, 3), index=['a', 'c', 'e'],columns=['one',
'two', 'three'])
df = df.reindex(['a', 'b', 'c'])
print(df)
#用 0 填充 NaN
print (df.fillna(0))
```

输出结果：

```
        one       two    three
a -1.536464  0.011786  0.58957
b       NaN       NaN      NaN
c -0.706135 -0.127373  1.10970

        one       two    three
a -1.536464  0.011786  0.58957
b  0.000000  0.000000  0.00000
c -0.706135 -0.127373  1.10970
```

当然根据您自己的需求，您也可以用其他值进行填充。

#### 2) 向前和向后填充NA

在Pandas reindex重置索引中，介绍了 ffill() 向前填充和 bfill() 向后填充，使用这两个函数也可以处理 NA 值。示例如下：

```python
df = df.reindex(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'])
print(df)
print(df.fillna(method='ffill'))
```

输出结果：

```
        one       two    three
a -1.536464  0.011786  0.58957
b       NaN       NaN      NaN
c -0.706135 -0.127373  1.10970
d       NaN       NaN      NaN
e       NaN       NaN      NaN
f       NaN       NaN      NaN
g       NaN       NaN      NaN
h       NaN       NaN      NaN

        one       two    three
a -1.536464  0.011786  0.58957
b -1.536464  0.011786  0.58957
c -0.706135 -0.127373  1.10970
d -0.706135 -0.127373  1.10970
e -0.706135 -0.127373  1.10970
f -0.706135 -0.127373  1.10970
g -0.706135 -0.127373  1.10970
h -0.706135 -0.127373  1.10970
```

或者您也可以采用向后填充的方法。

#### 3) 使用replace替换通用值

在某些情况下，您需要使用 replace() 将 DataFrame 中的通用值替换成特定值，这和使用 fillna() 函数替换 NaN 值是类似的。示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame({'one':[10,20,30,40,50,666], 'two':[99,0,30,40,50,60]})
print(df)
#使用replace()方法
print (df.replace({99:10,666:60,0:20}))
```

输出结果：

```
   one  two
0   10   99
1   20    0
2   30   30
3   40   40
4   50   50
5  666   60

   one  two
0   10   10
1   20   20
2   30   30
3   40   40
4   50   50
5   60   60
```

## 删除缺失值

如果想删除缺失值，那么使用 dropna() 函数与参数 axis 可以实现。在默认情况下，按照 axis=0 来按行处理，这意味着如果某一行中存在 NaN 值将会删除整行数据。示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5, 3), index=['a', 'c', 'e', 'f','h'],columns=['one', 'two', 'three'])
df = df.reindex(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'])
print(df)
#删除缺失值
print (df.dropna())
```

输出结果：

```
        one       two     three
a  0.043563 -0.817784 -0.083797
b       NaN       NaN       NaN
c  0.506784 -2.020886 -0.500083
d       NaN       NaN       NaN
e -0.197938 -0.200571  1.032237
f -0.781612  0.387806 -1.256790
g       NaN       NaN       NaN
h  0.481811  1.004817  0.062243

        one       two     three
a  0.043563 -0.817784 -0.083797
c  0.506784 -2.020886 -0.500083
e -0.197938 -0.200571  1.032237
f -0.781612  0.387806 -1.256790
h  0.481811  1.004817  0.062243
```

axis = 1 表示按列处理，处理结果是一个空的 DataFrame 对象。