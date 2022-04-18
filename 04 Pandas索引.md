# 04 Pandas 索引

## 列索引操作DataFrame

DataFrame 可以使用列索（columns index）引来完成数据的选取、添加和删除操作。这里用 df 代表pd.DataFrame(数据) ,如下表：

| 输入                             | 输出                                      |
| -------------------------------- | ----------------------------------------- |
| df.列名                          | 取出**单列**，输出格式为Series            |
| df[列名]                         | 取出**单列**，输出格式为Series            |
| df[ [列名] ]                     | 取出单列，输出格式为DataFrame             |
| df[ [列名1，列名2, 列名3， ……] ] | 取出各个列对应的元素，输出格式为DataFrame |

#### 1) 列索引选取数据列

您可以使用列索引，轻松实现数据选取，示例如下：

```python
import pandas as pd
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
print(df ['one'])
```

输出结果：

```
a     1.0
b     2.0
c     3.0
d     NaN
Name: one, dtype: float64
```

列的索引也可以使用loc()方法，这在下一节行操作中将会着重介绍，示例如下：

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

#### 2) 列索引添加数据列

使用 columns 列索引表标签可以实现添加新的数据列，示例如下：

```python
import pandas as pd
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
#使用df['列']=值，插入新的数据列
df['three']=pd.Series([10,20,30],index=['a','b','c'])
print(df)
#将已经存在的数据列做相加运算
df['four']=df['one']+df['three']
print(df)
```

输出结果：

```
使用列索引创建新数据列:
     one   two   three
a    1.0    1    10.0
b    2.0    2    20.0
c    3.0    3    30.0
d    NaN    4    NaN

已存在的数据列做算术运算：
      one   two   three    four
a     1.0    1    10.0     11.0
b     2.0    2    20.0     22.0
c     3.0    3    30.0     33.0
d     NaN    4     NaN     NaN
```

上述示例，我们初次使用了 DataFrame 的算术运算，这和 NumPy 非常相似。除了使用`df[]=value`的方式外，您还可以使用 insert() 方法插入新的列，示例如下：

```python
import pandas as pd
info=[['Jack',18],['Helen',19],['John',17]]
df=pd.DataFrame(info,columns=['name','age'])
print(df)
#注意是column参数
#数值1代表插入到columns列表的索引位置
df.insert(1,column='score',value=[91,90,75])
print(df)
```

输出结果：

```
添加前：
    name  age
0   Jack   18
1  Helen   19
2   John   17

添加后：
    name  score  age
0   Jack     91   18
1  Helen     90   19
2   John     75   17
```

#### 3) 列索引删除数据列

通过 del 和 pop() 都能够删除 DataFrame 中的数据列。del方法不会返回任何值，但是pop会以series格式返回删除的列示例如下：

```python
import pandas as pd
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd']),   'three' : pd.Series([10,20,30], index=['a','b','c'])}
df = pd.DataFrame(d)
print ("Our dataframe is:")
print(df)
#使用del删除
del df['one']
print(df)
#使用pop方法删除
df.pop('two')
print (df)
```

输出结果：

```
原DataFrame:
      one   three  two
a     1.0    10.0   1
b     2.0    20.0   2
c     3.0    30.0   3
d     NaN     NaN   4

使用del删除:
      three    two
a     10.0     1
b     20.0     2
c     30.0     3
d     NaN      4

使用 pop()删除:
   three
a  10.0
b  20.0
c  30.0
d  NaN
```

## 行索引操作DataFrame

理解了上述的列索引操作后，行索引操作就变的简单。下面看一下，如何使用行索引来选取 DataFrame 中的数据。

#### 1) 标签索引选取(loc)

df.loc[] 只能使用标签索引，不能使用整数索引。当通过标签索引的切片方式来筛选数据时，它的取值前闭后闭，也就是只包括边界值标签（开始和结束）。

loc[] 具有多种访问方法，如下所示：

- 一个标量标签
- 标签列表
- 切片对象
- 布尔数组


loc[] 接受两个参数，并以`','`分隔。第一个位置表示行，第二个位置表示列。方法总结：

```python
#有一个参数 
df.loc[行索引]   
df.loc[ [index] ]
df.loc[ [index1， index2, index3 ， ……] ] 	
#两个参数，横纵联合 
df.loc[row_label, col_label]
#同样的也有
df.loc[ [row_label1, row_label2, ……], [col_label1, col_label2, ……]]
```

示例如下：

```python
import pandas as pd
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
print(df)
print(df.loc['b'])
print(df.loc['b']['one'])
print(df.loc[['b','a']])
```

输出结果：

```
   one  two
a  1.0    1
b  2.0    2
c  3.0    3
d  NaN    4

one    2.0
two    2.0
Name: b, dtype: float64

2.0

   one  two
b  2.0    2
a  1.0    1
```

注意：loc 允许接两个参数分别是行和列，参数之间需要使用“逗号”隔开，但**该函数只能接收标签索引**。

选取多个行的示例如下：

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

#### 2) 整数索引选取(iloc)

df.iloc[] 只能使用整数索引，不能使用标签索引，通过整数索引切片选择数据时，前闭后开(不包含边界结束值)。同 Python 和 NumPy 一样，它们的索引都是从 0 开始。

.iloc[] 提供了以下方式来选择数据：

  1) 整数索引
  2) 整数列表
  3) 数值范围


示例如下：

```python
import pandas as pd
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
print (df.iloc[2]）
```

输出结果：

```
one   3.0
two   3.0
Name: c, dtype: float64
```

注意：iloc 允许接受两个参数分别是行和列，参数之间使用“逗号”隔开，但该函数只能接收整数索引。

也可以同时选择行和列，示例如下：

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

#### 3) 切片操作多行选取

您也可以使用切片的方式同时选取多行。示例如下：

```
import pandas as pd
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
#左闭右开
print(df[2:4])
#使用iloc也是相同效果
print(df.iloc[2:4])
```

输出结果：

```
   one  two
c  3.0    3
d  NaN    4
```

#### 4) 布尔索引选取

DataFrame可根据条件进行筛选，当条件判断True时，返回。当条件判断为False时，过滤掉。方法总结：

```python
#如果条件为真，则选出符合条件的结果。
df.loc[condition] 
# 复合条件：  &（且 = 与） , |（或） ,  ~（取反 = 非） 
# 通过筛选函数进行筛选， 常见的有 isin(), where() 等
```

示例如下：

```python
import pandas as pd
df = pd.DataFrame(data=[[22, "United Kingdom", "Lawyer" ],
                        [26, "China", "Engineer"],
                        [24, "United States", "Teacher"],
                        [32, "France", "Doctor"]],
                  index=pd.Index(["Chris", "Eric", "Mike", "Marry"], name="Name"),
                  columns=["Age", "Nation", "Profession"])
#等式条件
print(df.loc[df.index == "Chris"])     
#不等式条件
print(df["Age"].loc[df["Age"] > 25])    
#混合条件
con = (df["Nation"] == 'United Kingdom') & (df["Age"] > 20)
print(df.loc[con])
#isin()方法
print(df.loc[df.index.isin(['Chris','Eric'])])   
```

输出结果：

```
       Age          Nation Profession
Name                                 
Chris   22  United Kingdom     Lawyer

Name
Eric     26
Marry    32
Name: Age, dtype: int64

       Age          Nation Profession
Name                                 
Chris   22  United Kingdom     Lawyer

       Age          Nation Profession
Name                                 
Chris   22  United Kingdom     Lawyer
Eric    26           China   Engineer
```

#### 5) query()选取

query()函数是一种更简洁的使用布尔表达式查询的方法：

```
df[df["列名"] =="条件"] 等价于 df.query("列名 == '条件'")
```

示例如下：

```python
import pandas as pd
df = pd.DataFrame(data=[[22, "United Kingdom", "Lawyer" ],
                        [26, "China", "Engineer"],
                        [24, "United States", "Teacher"],
                        [32, "France", "Doctor"]],
                  index=pd.Index(["Chris", "Eric", "Mike", "Marry"], name="Name"),
                  columns=["Age", "Nation", "Profession"])
#等式条件
print(df.query("Age == 22"))     
#不等式条件
print(df.query("Age > 25"))  
# 混合条件
print(df.query("Nation == 'United Kingdom' & Age > 20"))  
```

输出结果：

```
       Age          Nation Profession
Name                                 
Chris   22  United Kingdom     Lawyer

       Age  Nation Profession
Name                         
Eric    26   China   Engineer
Marry   32  France     Doctor

       Age          Nation Profession
Name                                 
Chris   22  United Kingdom     Lawyer
```

#### 6) 添加数据行

使用 append() 函数，可以将新的数据行添加到 DataFrame 中，该函数会在行末追加数据行。示例如下：

```python
import pandas as pd
df = pd.DataFrame([[1, 2], [3, 4]], columns = ['a','b'])
df2 = pd.DataFrame([[5, 6], [7, 8]], columns = ['a','b'])
#在行末追加新数据行
df = df.append(df2, ignore_index=True)
print(df)
#使用concat方法
df = pd.concat([df,df2])
print(df)
```

输出结果：

```
   a  b
0  1  2
1  3  4
3  5  6
4  7  8
```

#### 7) 删除数据行

您可以使用行索引标签，从 DataFrame 中删除某一行数据。如果索引标签存在重复，那么它们将被一起删除。示例如下：

```python
import pandas as pd
df = pd.DataFrame([[1, 2], [3, 4], [5, 6]], columns = ['a','b'])
print(df)
#此处调用了drop()方法
df = df.drop(2)
print (df)
```

输出结果：

```
执行drop(0)前：
   a  b
0  1  2
1  3  4
2  5  6

执行drop(0)后：
   a  b
0  1  2
1  3  4
```

在上述的示例中，默认使用 range(2) 生成了行索引，并通过 drop(0) 同时删除了两行数据。
