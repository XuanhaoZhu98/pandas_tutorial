# 12 Pandas loc/iloc用法详解

在数据分析过程中，很多时候需要从数据表中提取出相应的数据，而这么做的前提是需要先“索引”出这一部分数据。虽然通过 Python 提供的索引操作符`"[]"`和属性操作符`"."`可以访问 Series 或者 DataFrame 中的数据，但这种方式只适应与少量的数据，为了解决这一问题，Pandas 提供了两种类型的索引方式来实现数据的访问。

本节就来讲解一下，如何在 Pandas 中使用 loc 函数和 iloc 函数。两种函数说明如下：

| 方法名称 | 说明                     |
| -------- | ------------------------ |
| .loc[]   | 基于**标签索引**选取数据 |
| .iloc[]  | 基于整数索引选取数据     |

## .loc[]

df.loc[] 只能使用标签索引，不能使用整数索引。当通过标签索引的切片方式来筛选数据时，它的取值前闭后闭，也就是只包括边界值标签（开始和结束）。

.loc[] 具有多种访问方法，如下所示：

- 一个标量标签
- 标签列表
- 切片对象
- 布尔数组


loc[] 接受两个参数，并以`','`分隔。第一个位置表示行，第二个位置表示列。示例如下：

```python
import numpy as np
import pandas as pd
#创建一组数据
data = {'name': ['John', 'Mike', 'Mozla', 'Rose', 'David', 'Marry', 'Wansi', 'Sidy', 'Jack', 'Alic'],
        'age': [20, 32, 29, np.nan, 15, 28, 21, 30, 37, 25],
        'gender': [0, 0, 1, 1, 0, 1, 0, 0, 1, 1],
        'isMarried': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}
label = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=label)
print(df)
#对行操作
print(df.loc['a':'d',:]) #等同于df.loc['a':'d']
```

输出结果：

```
    name   age  gender isMarried
a   John  20.0       0       yes
b   Mike  32.0       0       yes
c  Mozla  29.0       1        no
d   Rose   NaN       1       yes
e  David  15.0       0        no
f  Marry  28.0       1        no
g  Wansi  21.0       0        no
h   Sidy  30.0       0       yes
i   Jack  37.0       1        no
j   Alic  25.0       1        no
#从a到d,切记包含d
    name   age  gender isMarried
a   John  20.0       0       yes
b   Mike  32.0       0       yes
c  Mozla  29.0       1        no
d   Rose   NaN       1       yes
```

对列进行操作，示例如下：

```python
import numpy as np
import pandas as pd
#创建一组数据
data = {'name': ['John', 'Mike', 'Mozla', 'Rose', 'David', 'Marry', 'Wansi', 'Sidy', 'Jack', 'Alic'],
        'age': [20, 32, 29, np.nan, 15, 28, 21, 30, 37, 25],
        'gender': [0, 0, 1, 1, 0, 1, 0, 0, 1, 1],
        'isMarried': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}
label = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=label)
print(df.loc[:,'name'])
```

输出结果：

```
a     John
b     Mike
c    Mozla
d     Rose
e    David
f    Marry
g    Wansi
h     Sidy
i     Jack
j     Alic
Name: name, dtype: object
```

对行和列同时操作，示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(8, 4),
index = ['a','b','c','d','e','f','g','h'], columns = ['A', 'B', 'C', 'D'])
print(df.loc[['a','b','f','h'],['A','C']])
```

输出如下：

```
          A         C
a  1.168658  0.008070
b -0.076196  0.455495
f  1.224038  1.234725
h  0.050292 -0.031327
```

布尔值操作，示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(4, 4),index = ['a','b','c','d'], columns = ['A', 'B', 'C', 'D'])
#返回一组布尔值
print(df.loc['b']>0)
```

输出结果：

```
A     True
B     True
C    False
D     True
Name: b, dtype: bool
```

## .iloc[]

df.iloc[] 只能使用整数索引，不能使用标签索引，通过整数索引切片选择数据时，前闭后开(不包含边界结束值)。同 Python 和 NumPy 一样，它们的索引都是从 0 开始。

.iloc[] 提供了以下方式来选择数据：
 1) 整数索引
 2) 整数列表
 3) 数值范围


示例如下：

```python
data = {'name': ['John', 'Mike', 'Mozla', 'Rose', 'David', 'Marry', 'Wansi', 'Sidy', 'Jack', 'Alic'],
        'age': [20, 32, 29, np.nan, 15, 28, 21, 30, 37, 25],
        'gender': [0, 0, 1, 1, 0, 1, 0, 0, 1, 1],
        'isMarried': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}
label = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
df = pd.DataFrame(data, index=label)
print(df)
print(df.iloc[2,])
```

输出结果：

```
    name   age  gender isMarried
a   John  20.0       0       yes
b   Mike  32.0       0       yes
c  Mozla  29.0       1        no
d   Rose   NaN       1       yes
e  David  15.0       0        no
f  Marry  28.0       1        no
g  Wansi  21.0       0        no
h   Sidy  30.0       0       yes
i   Jack  37.0       1        no
j   Alic  25.0       1        no

name         Mozla
age             29
gender           1
isMarried       no
Name: c, dtype: object
```

再看一组示例：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(8, 4), columns = ['A', 'B', 'C', 'D'])
print df.iloc[[1, 3, 5], [1, 3]]
print df.iloc[1:3, :]
print df.iloc[:,1:3]
```

输出结果：

```
          B         D
1  0.773595 -0.206061
3 -1.740403 -0.464383
5  1.046009  0.606808

          A         B         C         D
1 -0.093711  0.773595  0.966408 -0.206061
2 -1.122587 -0.135011  0.546475 -0.551403

          B         C
0  0.623488  3.328406
1  0.773595  0.966408
2 -0.135011  0.546475
3 -1.740403 -0.869073
4  0.591573 -1.463275
5  1.046009  2.330035
6 -0.266607  0.873971
7 -1.059625 -0.405340
```