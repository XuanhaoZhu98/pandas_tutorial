# 13 Pandas处理字符串

Pandas 提供了一系列的字符串函数，因此能够很方便地对字符串进行处理。在本节，我们使用 Series 对象对常用的字符串函数进行讲解。常用的字符串处理函数如下表所示：

| 函数名称            | 函数功能和描述                                               |
| ------------------- | ------------------------------------------------------------ |
| lower()             | 将字符串转换为小写。                                         |
| upper()             | 将字符串转换为大写。                                         |
| len()               | 得出字符串的长度。                                           |
| strip()             | 去除字符串两边的空格（包含换行符）。                         |
| split()             | 用指定的分割符分割字符串。                                   |
| cat(sep="")         | 用给定的分隔符连接字符串元素。                               |
| get_dummies()       | 返回一个带有独热编码值的 DataFrame 结构。                    |
| contains(pattern)   | 如果子字符串包含在元素中，则为每个元素返回一个布尔值 True，否则为 False。 |
| replace(a,b)        | 将值 a 替换为值 b。                                          |
| count(pattern)      | 返回每个字符串元素出现的次数。                               |
| startswith(pattern) | 如果 Series 中的元素以指定的字符串开头，则返回 True。        |
| endswith(pattern)   | 如果 Series 中的元素以指定的字符串结尾，则返回 True。        |
| findall(pattern)    | 以列表的形式返出现的字符串。                                 |
| swapcase()          | 交换大小写。                                                 |
| islower()           | 返回布尔值，检查 Series 中组成每个字符串的所有字符是否都为小写。 |
| issupper()          | 返回布尔值，检查 Series 中组成每个字符串的所有字符是否都为大写。 |
| isnumeric()         | 返回布尔值，检查 Series 中组成每个字符串的所有字符是否都为数字。 |
| repeat(value)       | 以指定的次数重复每个元素。                                   |
| find(pattern)       | 返回字符串第一次出现的索引位置。                             |

注意：上述所有字符串函数全部适用于 DataFrame 对象，同时也可以与 Python 内置的字符串函数一起使用，这些函数在处理 Series/DataFrame 对象的时候会自动忽略缺失值数据（NaN）。

#### lower() 将字符串转换为小写

```python
import pandas as pd
import numpy as np
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.lower())
```

输出结果：

```
0             c
1        python
2          java
3            go
4           NaN
5           NaN
6    javascript
dtype: object
```

#### len() 返回字符串的长度

```py
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.lower())
```

输出结果：

```
0     1.0
1     6.0
2     4.0
3     2.0
4     NaN
5     NaN
6    10.0
dtype: float64
```

#### strip() 去除字符串两边的空格

```python
s = pd.Series([' C', 'Python ', ' JAVA ', 'go', np.nan, 2022,'javascript'])
print(s.str.strip())
```

输出结果：

```
0             C
1        Python
2          JAVA
3            go
4           NaN
5           NaN
6    javascript
dtype: object
```

#### split(pattern) 用指定的分割符分割字符串

```python
s = pd.Series(['C C++', 'Python', 'JAVA', 'go', np.nan, 2022,'html css javascript'])
print(s.str.split(" "))
```

输出结果：

```
0                   [C, C++]
1                   [Python]
2                     [JAVA]
3                       [go]
4                        NaN
5                        NaN
6    [html, css, javascript]
dtype: object

```

#### cat(sep="") 用给定的分隔符连接字符串

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan,'javascript'])
#会自动忽略NaN
print(s.str.cat(sep="_"))
```

输出结果：

```
C_Python_JAVA_go_javascript
```

#### get_dummies() 转换为独热编码

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan,'javascript'])
print(s.str.get_dummies())
```

输出结果：

```
   C  JAVA  Python  go  javascript
0  1     0       0   0           0
1  0     0       1   0           0
2  0     1       0   0           0
3  0     0       0   1           0
4  0     0       0   0           0
5  0     0       0   0           1
```

#### contains() 检查字符串包含

检查 Series 中的每个字符，如果字符中包含空格，则返回 True，否则返回 False。示例如下：

```python
s = pd.Series(['C ', 'Python', ' JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.contains(" "))
```

输出结果：

```
0     True
1    False
2     True
3    False
4      NaN
5      NaN
6    False
dtype: object
```

#### repeat() 以指定的次数重复每个元素

```python
s = pd.Series(['C ', 'Python', ' JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.repeat(3))
```

输出结果：

```
0                            C C C 
1                PythonPythonPython
2                    JAVA JAVA JAVA
3                            gogogo
4                               NaN
5                               NaN
6    javascriptjavascriptjavascript
dtype: object
```

#### startswith() 判断是否以指定的字符串开头

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
#若以指定的"j"开头则返回True
print(s.str.startswith("j"))
```

输出结果：

```
0    False
1    False
2    False
3    False
4      NaN
5      NaN
6     True
dtype: object
```

#### find() 返回字符串第一次出现的索引位置

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.find("j"))
```

输出结果：

```
0   -1.0
1   -1.0
2   -1.0
3   -1.0
4    NaN
5    NaN
6    0.0
dtype: float64
```

如果返回 -1 表示该字符串中没有出现指定的字符。

#### findall() 以列表的形式返出现的字符串

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.findall("j"))
```

输出结果：

```
0     []
1     []
2     []
3     []
4    NaN
5    NaN
6    [j]
dtype: object
```

#### replace(a,b)  将值 a 替换为值 b

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.replace("j", "J"))
```

输出结果：

```
0             C
1        Python
2          JAVA
3            go
4           NaN
5           NaN
6    Javascript
dtype: object
```

#### swapcase() 交换大小写

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, 2022,'javascript'])
print(s.str.swapcase())
```

输出结果：

```
0             c
1        pYTHON
2          java
3            GO
4           NaN
5           NaN
6    JAVASCRIPT
dtype: object
```

#### isnumeric() 是否所有字符是否都为数字

返回一个布尔值，用来判断是否存在数字型字符串。示例如下：

```python
s = pd.Series(['C', 'Python', 'JAVA', 'go', np.nan, '2022','javascript'])
print(s.str.isnumeric())
```

输出结果：

```
0    False
1    False
2    False
3    False
4      NaN
5     True
6    False
dtype: object
```