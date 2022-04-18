# 07 Pandas分层索引

分层索引（Multiple Index）是 Pandas 中非常重要的索引类型，它指的是在一个轴上拥有多个（即两个以上）索引层数，这使得我们可以用低维度的结构来处理更高维的数据。比如，当想要处理三维及以上的高维数据时，就需要用到分层索引。

分层索引的目的是用低维度的结构（Series 或者 DataFrame）更好地处理高维数据。通过分层索引，我们可以像处理二维数据一样，处理三维及以上的数据。分层索引的存在使得分析高维数据变得简单，让抽象的高维数据变得容易理解，同时它比废弃的 Panel 结构更容易使用。

Pandas 可以通过 MultiIndex() 方法来创建分层索引对象，该对象本质上是一个元组序列，序列中每一个元组都是唯一的。下面介绍几种创建分层索引的方式。

## 创建分层索引

#### 1) 从数组创建

通过 MultiIndex() 的`levels`参数能够直接创建分层索引，该函数的常用参数项，如下表所示：

| 参数名称  | 描述说明                               |
| --------- | -------------------------------------- |
| levels    | 每个级的唯一标签。                     |
| codes     | 每个级的索引，指定每个位置的标签。     |
| sortorder | 排序级（必须按该级别按字典顺序排序）。 |
| names     | 每个索引级别的名称。                   |

示例如下：

```python
import pandas as pd 
import numpy as np 
#为leves传递一个2行6列的二维数组
array = [[1, 2, 3, np.nan, pd.NaT, None],['red', 'blue', 'green', 'black', 'yellow', 'white']]
codes = [[5, 4, 3, 2, 0],[1,2,3,4,-1]]
df=pd.MultiIndex(levels = array, codes = codes) 
print(df)
```

输出结果：

```
MultiIndex([(nan,   'blue'),
            (nan,  'green'),
            (nan,  'black'),
            (  3, 'yellow'),
            (  1,      nan)],
           )
```

上述代码中，`levels`参数用来创建层级索引，`codes`表示按参数值对层级索引值排序（与 levels 中的值相对应），也就说 codes 中数值是 leves 序列的下标索引。需要注意，这里的 -1 代表 NaN。

也可以不指定codes直接创建：

```python
arrays = [[1, 2, 3, 4], ['red', 'blue', 'red', 'blue']]
df=pd.MultiIndex.from_arrays(arrays, names=('number', 'color'))
print(df)
```

输出结果：

```
MultiIndex([(1,  'red'),
            (2, 'blue'),
            (3,  'red'),
            (4, 'blue')],
           names=['number', 'color'])
```

可以看到，这边的索引和二维数组相互对应

#### 2) 从元组创建

通过 from_tuples() 实现从元组序列创建分层索引。

直接创建元组序列：

```python
tuples = [(1, 'red'), (1, 'blue'),(2, 'red'), (2, 'blue')]
index = pd.MultiIndex.from_tuples(tuples, names=('number', 'color'))
print(index)
```

输出结果：

```
MultiIndex([(1,  'red'),
            (1, 'blue'),
            (2,  'red'),
            (2, 'blue')],
           names=['number', 'color'])
```

使用zip()函数创建元组序列：

```python
#创建元组序列
arrays = [['it', 'it', 'of', 'of', 'for', 'for', 'then', 'then'], 
['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']] 
#使用zip()函数创建元组
tuples = list(zip(*arrays)) 
print(tuples)
```

输出结果：

```
[('it', 'one'), ('it', 'two'), ('of', 'one'), ('of', 'two'), ('for', 'one'), ('for', 'two'), ('then', 'one'), ('then', 'two')]
```

然后使用 tuples 创建分层索引，如下所示：

```python
import pandas as pd
#创建了两层索引，并使用names对它们命名 
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
print(index)
```

输出结果：

```
MultiIndex([(  'it', 'one'),
            (  'it', 'two'),
            (  'of', 'one'),
            (  'of', 'two'),
            ( 'for', 'one'),
            ( 'for', 'two'),
            ('then', 'one'),
            ('then', 'two')],
           names=['first', 'second'])
```

#### 3) 从DataFrame对象创建

通过 from_frame() 创建分层索引，示例如下：

```python
#首先创建一个 DataFrame。
import pandas as pd
df = pd.DataFrame([['bar', 'one'], ['bar', 'two'],
                   ['foo', 'one'], ['foo', 'two']],
                  columns=['first', 'second'])
#然后使用 from_frame()创建分层索引。
index = pd.MultiIndex.from_frame(df)
print(index)
#将index应用于Series
s=pd.Series(np.random.randn(4), index=index)
print(s)
```

输出结果：

```
MultiIndex([('bar', 'one'),
            ('bar', 'two'),
            ('foo', 'one'),
            ('foo', 'two')],
           names=['first', 'second'])
           
first  second
bar    one      -0.533818
       two      -0.806120
foo    one       1.723898
       two       0.937780
dtype: float64
```

#### 4) 笛卡尔积创建

笛卡尔积（又称直积）是数学运算的一种方式，下面使用 from_product() 笛卡尔积创建分层索引。

```python
import pandas as pd
#构建数据
numbers = [0, 1, 2]
language = ['Python', 'Java']
#经过笛卡尔积处理后会得到6中组合方式
index = pd.MultiIndex.from_product([numbers, language],names=['number', 'language'])
print(index)
#将分层索引对象应用于Series
dk_er=pd.Series(np.random.randn(6), index=index)
print(dk_er)
```

输出结果：

```
MultiIndex([(0, 'Python'),
            (0,   'Java'),
            (1, 'Python'),
            (1,   'Java'),
            (2, 'Python'),
            (2,   'Java')],
           names=['number', 'language'])
           
number  language
0       Python      0.516328
        Java       -2.341224
1       Python      0.098108
        Java        0.141528
2       Python     -0.488959
        Java        1.038360
dtype: float64
```

## 应用分层索引

下面示例讲解了如何在 DataFrame 中应用分层索引。

```python
import pandas as pd 
import numpy as np
#创建一个数组
arrays = [[0, 0, 1, 1], ['A', 'B', 'A', 'B']]
#从数组创建
index=pd.MultiIndex.from_arrays(arrays, names=('number', 'letter'))
print(index)
```

输出结果

```
MultiIndex([(0, 'A'),
            (0, 'B'),
            (1, 'A'),
            (1, 'B')],
           names=['number', 'letter'])
```

上述示例中，第一层为 number，该层有 0、1 两个元素，第二层为 letter，有两个字母 A 和 B。

下面把已经创建的分层索引应用到 DataFrame 中，如下所示：

```python
#在行索引位置应用分层索引
df=pd.DataFrame([{'a':11, 'b':22}], index=index)
print(df)
```

输出结果：

```
                a   b
number letter       
0      A       11  22
       B       11  22
1      A       11  22
       B       11  22
```

通过 set_index() 可以将 DataFrame 的已有列的标索设置为 index 行索引，示例如下：

```python
import pandas as pd
df= pd.DataFrame({'a': range(5), 'b': range(5, 0, -1),
                  'c': ['one', 'one', 'one', 'two', 'two'],
                  'd': [0, 1, 2, 0, 1]})
print(df)
df1=df.set_index(['a','d'],drop=False)
print(df1)
df2=df.set_index(['a','d'],drop=True,append=True)
print(df2)
```

输出结果：

```
   a  b    c  d
0  0  5  one  0
1  1  4  one  1
2  2  3  one  2
3  3  2  two  0
4  4  1  two  1

     a  b    c  d
a d              
0 0  0  5  one  0
1 1  1  4  one  1
2 2  2  3  one  2
3 0  3  2  two  0
4 1  4  1  two  1

       b    c
  a d        
0 0 0  5  one
1 1 1  4  one
2 2 2  3  one
3 3 0  2  two
4 4 1  1  two
```

通过 set_index() 将列索引转换为了分层行索引，其中 drop=False 表示更新索引的同时，不删除 a、d 列；同时，该函数还提供了一个 append = Ture 参数表示不添加默认的整数索引值（0到4）

## 分层索引切片取值

下面讲解分层索引切片取值操作，示例如下：

#### 1) 分层行索引操作

```python
import pandas as pd
#构建多层索引
tuple = [('C',2021),('JAVA',2021),
      ('C',2022),('Python',2022),
   ('Go',2007),('PHP',2008)]
salary = [18000,20000,19000,25000,19000,22000]
#其次应用于DataFrame
index = pd.MultiIndex.from_tuples(tuple)
s = pd.Series(salary, index=index)
print(s)
#切片取值
print(s['C',2021])
print(s['C'])
print(s[:,2022])
#比较value
print(s[s<=20000])
```

输出结果：

```
C       2021    18000
JAVA    2021    20000
C       2022    19000
Python  2022    25000
Go      2007    19000
PHP     2008    22000
dtype: int64

18000

2021    18000
2022    19000
dtype: int64

C         19000
Python    25000
dtype: int64

C     2021    18000
JAVA  2021    20000
C     2022    19000
Go    2007    19000
dtype: int64
```

#### 2) 行、列多层索引操作

下面看一种更加复杂的情况，就是行、列同时存在多层索引时候，应该如何通过切片取值。示例如下：

```python
df = pd.DataFrame(np.arange(1,17).reshape((4, 4)),
               index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
               columns=[['Jack', 'Jack', 'Helen', 'Eric'],
              ['Python', 'Java', 'Python', 'C++']])
print(df)
#选择同一层级的索引,切记不要写成['Jack','Helen']
print(df[['Jack','Helen']])
#在不同层级分别选择索引
print(df['Jack','Python'])
#iloc整数索引
print(df.iloc[:3,:2])
#loc列标签索引
print(df.loc[:,('Helen','Python')])
```

输出结果：

```
      Jack       Helen Eric
    Python Java Python  C++
a 1      1    2      3    4
  2      5    6      7    8
b 1      9   10     11   12
  2     13   14     15   16
  
      Jack       Helen
    Python Java Python
a 1      1    2      3
  2      5    6      7
b 1      9   10     11
  2     13   14     15
  
a  1     1
   2     5
b  1     9
   2    13
Name: (Jack, Python), dtype: int32

      Jack     
    Python Java
a 1      1    2
  2      5    6
b 1      9   10

a  1     3
   2     7
b  1    11
   2    15
Name: (Helen, Python), dtype: int32
```

## 聚合函数应用

通过给`level`传递参数值，您可以指定在哪个层上进行聚合操作，比如求和、求均值等。示例如下：

```python
df = pd.DataFrame(np.arange(1,17).reshape((4, 4)),
               index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
               columns=[['Jack', 'Jack', 'Helen', 'Eric'],
              ['Python', 'Java', 'Python', 'C++']])
#第一步，给行列层级起名字
df.index.names=['key1','key2']
df.columns.names=['name','course']
print(df)
print(df.groupby(level='key2').sum())
print(df.groupby(level="course",axis=1).mean())
```

输出结果：

```
name        Jack       Helen Eric
course    Python Java Python  C++
key1 key2                        
a    1         1    2      3    4
     2         5    6      7    8
b    1         9   10     11   12
     2        13   14     15   16
     
name     Jack       Helen Eric
course Python Java Python  C++
key2                          
1          10   12     14   16
2          18   20     22   24

course      C++  Java  Python
key1 key2                    
a    1      4.0   2.0     2.0
     2      8.0   6.0     6.0
b    1     12.0  10.0    10.0
     2     16.0  14.0    14.0
```

在数据分析的过程中，我们把大部分时间都花费在数据的准备和预处理上，Pandas 作为一个灵活、高效的数据预处理工具，提供了诸多数据处理的方法，分层索引（Multiple Index）就是其中之一，分层索引（或多层索引）是 Pandas 的基本特性，它能够增强 Pands 数据预处理的能力。

## Series分层索引

对于 Series 结构来说，通过给`index`参数传递一个二维数组就可以创建一个具有两层索引的 MultiIndex 对象，示例如下：

```python
import pandas as pd 
info = pd.Series([11, 14, 17, 24, 19, 32, 34, 27],
index = [['x', 'x', 'x', 'x', 'y', 'y', 'y', 'y'],
['obj1', 'obj2', 'obj3', 'obj4', 'obj1', 'obj2', 'obj3', 'obj4']]) 
print(info)
```

输出结果：

```
x  obj1    11
   obj2    14
   obj3    17
   obj4    24
y  obj1    19
   obj2    32
   obj3    34
   obj4    27
dtype: int64
```

局部索引可以理解为：从分层索引中选择特定索引层的一种方法。比如在下列数据中，选择所有`'y'`索引指定的数据，示例如下：

```python
import pandas as pd 
info = pd.Series([11, 14, 17, 24, 19, 32, 34, 27], 
index = [['x', 'x', 'x', 'x', 'y', 'y', 'y', 'y'], 
['obj1', 'obj2', 'obj3', 'obj4', 'obj1', 'obj2', 'obj3', 'obj4']]) 
print(info)
print(info[:,'obj1'])
print(info['y'])
```

输出结果：

```
x  obj1    11
   obj2    14
   obj3    17
   obj4    24
y  obj1    19
   obj2    32
   obj3    34
   obj4    27
dtype: int64

x    11
y    19
dtype: int64

obj1    19
obj2    32
obj3    34
obj4    27
dtype: int64
```

## 行索引层转换为列索引

unstack() 用来将行索引转变成列索引，相当于转置操作。通过 unstack() 可以将 Series（一维序列）转变为 DataFrame（二维序列）。unstack(0) 表示选择第一层索引作为列，unstack(1) 表示选择第二层，示例如下：

```python
import pandas as pd 
info = pd.Series([11, 14, 17, 24, 19, 32, 34, 27], 
index = [['x', 'x', 'x', 'x', 'y', 'y', 'y', 'y'], 
['obj1', 'obj2', 'obj3', 'obj4', 'obj1', 'obj2', 'obj3', 'obj4']]) 
print(info)
#行索引标签默认是最外层的 x, y
#0代表第一层索引，而1代表第二层
print(info.unstack(0))
print(info.unstack(1))
```

输出结果：

```
x  obj1    11
   obj2    14
   obj3    17
   obj4    24
y  obj1    19
   obj2    32
   obj3    34
   obj4    27
dtype: int64

       x   y
obj1  11  19
obj2  14  32
obj3  17  34
obj4  24  27

   obj1  obj2  obj3  obj4
x    11    14    17    24
y    19    32    34    27
```

## 交换层和层排序

#### 1) 交换层

通过 swaplevel() 方法轻松地实现索引层交换，示例如下：

```python
import pandas as pd
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                  index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
                  columns=[['Ohio', 'Ohio', 'Colorado'],
                           ['Green', 'Red', 'Green']])
#设置index的levels名称                         
frame.index.names = ['key1', 'key2']
#设置columns的levels名称
frame.columns.names = ['state','color']
print(frame)
#交换key1层与key层
frame = frame.swaplevel('key1','key2')
print(frame)
#交换state和color层
frame = frame.swaplevel('state','color', axis = 1)
print(frame)
#使用索引标明要交换的层
frame = frame.swaplevel(0,1)
frame = frame.swaplevel(0,1, axis = 1)
print(frame)
```

输出结果：

```
state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
a    1        0   1        2
     2        3   4        5
b    1        6   7        8
     2        9  10       11
     
state      Ohio     Colorado
color     Green Red    Green
key2 key1                   
1    a        0   1        2
2    a        3   4        5
1    b        6   7        8
2    b        9  10       11

color     Green  Red    Green
state      Ohio Ohio Colorado
key2 key1                    
1    a        0    1        2
2    a        3    4        5
1    b        6    7        8
2    b        9   10       11

state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
a    1        0   1        2
     2        3   4        5
b    1        6   7        8
     2        9  10       11
```

使用reorder_levels调整多层顺序

```python
L1,L2,L3 = ['A','B'],['a','b'],['alpha','beta']
mul_index1 = pd.MultiIndex.from_product([L1,L2,L3], names=('Upper', 'Lower','Extra'))
L4,L5,L6 = ['C','D'],['c','d'],['cat','dog']
mul_index2 = pd.MultiIndex.from_product([L4,L5,L6], names=('Big', 'Small', 'animal'))
df = pd.DataFrame(np.random.randint(-9,10,(8,8)), index=mul_index1,  columns=mul_index2)
print(df)
#行索引改变顺序
df = df.reorder_levels([2,0,1], axis = 0) 
print(df)
```

输出结果：

```
Big                 C               D            
Small               c       d       c       d    
animal            cat dog cat dog cat dog cat dog
Upper Lower Extra                                
A     a     alpha   8  -5   1  -7  -8  -5  -9   1
            beta   -1  -4  -3  -8   0   1   6   8
      b     alpha   9  -8  -7  -6   6  -2   4  -9
            beta   -4  -4   1   6   4   2   0  -6
B     a     alpha   0  -5  -2   5   8  -3   0  -9
            beta   -3   0  -2  -3  -1  -8   3   1
      b     alpha  -5  -3   5   7  -1  -7   3   6
            beta   -2  -2  -5   1   4   5   6  -9
            
Big                 C               D            
Small               c       d       c       d    
animal            cat dog cat dog cat dog cat dog
Extra Upper Lower                                
alpha A     a       8  -5   1  -7  -8  -5  -9   1
beta  A     a      -1  -4  -3  -8   0   1   6   8
alpha A     b       9  -8  -7  -6   6  -2   4  -9
beta  A     b      -4  -4   1   6   4   2   0  -6
alpha B     a       0  -5  -2   5   8  -3   0  -9
beta  B     a      -3   0  -2  -3  -1  -8   3   1
alpha B     b      -5  -3   5   7  -1  -7   3   6
beta  B     b      -2  -2  -5   1   4   5   6  -9
```

#### 2) 层排序

通过 sort_index() 的`level`参数实现对层的排序。下面示例，按“key1”的字母顺序重新排序。

```python
import pandas as pd
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                  index=[['b', 'a', 'b', 'a'], [1, 2, 1, 2]],
                  columns=[['Ohio', 'Ohio', 'Colorado'],
                           ['Green', 'Red', 'Green']])
#设置index的levels的名称，key1 与 key2分别对应不同的层                         
frame.index.names = ['key1', 'key2']
#设置columns的levels的名称
frame.columns.names = ['state','color']
print(frame)
print(frame.sort_index(level='key1'))
```

输出结果：

```
state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
b    1        0   1        2
a    2        3   4        5
b    1        6   7        8
a    2        9  10       11

state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
a    2        3   4        5
     2        9  10       11
b    1        0   1        2
     1        6   7        8
```

## 层相关操作

#### 1) 修改索引层名

rename_axis用于修改索引层的名字，即在使用pd.MultiIndex创建时传入的names参数的值。可以传入字典进行修改：

```python
import pandas as pd
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                  index=[['b', 'a', 'b', 'a'], [1, 2, 1, 2]],
                  columns=[['Ohio', 'Ohio', 'Colorado'],
                           ['Green', 'Red', 'Green']])
#设置index的levels的名称，key1 与 key2分别对应不同的层                         
frame.index.names = ['key1', 'key2']
#设置columns的levels的名称
frame.columns.names = ['state','color']
frame = frame.rename_axis(index={'key1':'key_a'}, 
                  columns={'color':'colour'})
print(frame)
```

输出结果：

```
state       Ohio     Colorado
colour     Green Red    Green
key_a key2                   
b     1        0   1        2
a     2        3   4        5
b     1        6   7        8
a     2        9  10       11
```

#### 2) 修改索引值

rename对索引的值进行修改，如果是多级索引需要指定修改的层号level：

```python
import pandas as pd
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                  index=[['b', 'a', 'b', 'a'], [1, 2, 1, 2]],
                  columns=[['Ohio', 'Ohio', 'Colorado'],
                           ['Green', 'Red', 'Green']])
#设置index的levels的名称，key1 与 key2分别对应不同的层                         
frame.index.names = ['key1', 'key2']
#设置columns的levels的名称
frame.columns.names = ['state','color']
print(frame)
frame = frame.rename(index = {'a': 'c'}, level = 0)
print(frame)
```

输出结果：

```
state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
b    1        0   1        2
a    2        3   4        5
b    1        6   7        8
a    2        9  10       11

state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
b    1        0   1        2
c    2        3   4        5
b    1        6   7        8
c    2        9  10       11
```

#### 3) 删除层

若想要删除某一层的索引，可以使用droplevel方法。同时删除多层，可以传入一个列表：

```python
L1,L2,L3 = ['A','B'],['a','b'],['alpha','beta']
mul_index1 = pd.MultiIndex.from_product([L1,L2,L3], names=('Upper', 'Lower','Extra'))
L4,L5,L6 = ['C','D'],['c','d'],['cat','dog']
mul_index2 = pd.MultiIndex.from_product([L4,L5,L6], names=('Big', 'Small', 'animal'))
df = pd.DataFrame(np.random.randint(-9,10,(8,8)), index=mul_index1,  columns=mul_index2)
print(df)
df = df.droplevel([0,2], axis = 0)
print(df)
```

输出结果：

```
Big                 C               D            
Small               c       d       c       d    
animal            cat dog cat dog cat dog cat dog
Upper Lower Extra                                
A     a     alpha   1  -5   6  -7   6   9   8   8
            beta   -9  -7  -5   1   3  -4   1  -7
      b     alpha   0  -2  -7  -6   1  -4  -4  -1
            beta    2   8   7   0  -7   3   2  -6
B     a     alpha  -6  -1   8   4  -6  -4  -7   0
            beta   -6  -1  -5   1  -3   7  -4  -3
      b     alpha   8   7   5   9   0   2  -6  -2
            beta    1   7  -1  -6   4  -2  -4   9
            
Big      C               D            
Small    c       d       c       d    
animal cat dog cat dog cat dog cat dog
Lower                                 
a        1  -5   6  -7   6   9   8   8
a       -9  -7  -5   1   3  -4   1  -7
b        0  -2  -7  -6   1  -4  -4  -1
b        2   8   7   0  -7   3   2  -6
a       -6  -1   8   4  -6  -4  -7   0
a       -6  -1  -5   1  -3   7  -4  -3
b        8   7   5   9   0   2  -6  -2
b        1   7  -1  -6   4  -2  -4   9
```