# 19 Pandas concat连接操作

Pandas 通过 concat() 函数能够轻松地将 Series 与 DataFrame 对象组合在一起，函数的语法格式如下：

```python
pd.concat(objs,axis=0,join='outer',join_axes=None,ignore_index=False)
```

参数说明如下所示：

| 参数名称     | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| objs         | 一个序列或者是Series、DataFrame对象。                        |
| axis         | 表示在哪个轴方向上（行或者列）进行连接操作，默认 axis=0 表示行方向。 |
| join         | 指定连接方式，取值为{"inner","outer"}，默认为 outer 表示取并集，inner代表取交集。 |
| ignore_index | 布尔值参数，默认为 False，如果为 True，表示不在连接的轴上使用索引。 |
| join_axes    | 表示索引对象的列表。                                         |

## concat()

concat() 函数用于沿某个特定的轴执行连接操作。下面让我们创建不同的对象，并对其进行连接。，如果想让输出的行索引遵循依次递增的规则，那么需要将 ignore_index 设置为 True。

```python
import pandas as pd
a= pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3])
b= pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']},
                    index=[0, 1, 2, 3])
#连接a与b
print(pd.concat([a,b], ignore_index = True))
```

输出结果：

```
    A   B   C   D
0  A0  B0  C0  D0
1  A1  B1  C1  D1
2  A2  B2  C2  D2
3  A3  B3  C3  D3
4  A4  B4  C4  D4
5  A5  B5  C5  D5
6  A6  B6  C6  D6
7  A7  B7  C7  D7
```

如果想把指定的键与 DataFrame 对象连接，您可以使用 keys 参数来实现。如下所示：

```python
#连接a与b,并给a，b连接一个指定的键
print(pd.concat([a,b],keys=['x','y']))
```

输出结果：

```
      A   B   C   D
x 0  A0  B0  C0  D0
  1  A1  B1  C1  D1
  2  A2  B2  C2  D2
  3  A3  B3  C3  D3
y 0  A4  B4  C4  D4
  1  A5  B5  C5  D5
  2  A6  B6  C6  D6
  3  A7  B7  C7  D7
```

如果您想要沿着 axis=1 添加两个对象，那么将会追加新的列。

```python
print(pd.concat([a,b],axis=1))
```

输出结果：

```
    A   B   C   D   A   B   C   D
0  A0  B0  C0  D0  A4  B4  C4  D4
1  A1  B1  C1  D1  A5  B5  C5  D5
2  A2  B2  C2  D2  A6  B6  C6  D6
3  A3  B3  C3  D3  A7  B7  C7  D7
```

如果索引不同，则会用NaN进行填充。

```python
a= pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3])
b= pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D1', 'D2', 'D5', 'D6']},
                     index=[4, 5, 6, 7])
#沿着 axis=1，连接a与b
print(pd.concat([a,b],axis=1))
```

输出结果：

```
     A    B    C    D    A    B    C    D
0   A0   B0   C0   D0  NaN  NaN  NaN  NaN
1   A1   B1   C1   D1  NaN  NaN  NaN  NaN
2   A2   B2   C2   D2  NaN  NaN  NaN  NaN
3   A3   B3   C3   D3  NaN  NaN  NaN  NaN
4  NaN  NaN  NaN  NaN   A4   B4   C4   D1
5  NaN  NaN  NaN  NaN   A5   B5   C5   D2
6  NaN  NaN  NaN  NaN   A6   B6   C6   D5
7  NaN  NaN  NaN  NaN   A7   B7   C7   D6
```



## append()

注意：append()在1.4.0 版后已弃用，请改为使用concat()

如果要连接 Series 和 DataFrame 对象，有一个最方便、快捷的方法，那就是 append() 方法。该方法沿着 axis=0 （行方向）进行操作。

```python
import pandas as pd
a= pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3])
b= pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D1', 'D2', 'D5', 'D6']},
                     index=[4, 5, 6, 7])
#沿着 axis=0，使用 apppend()方法连接a与b
print(a.append(b, ignore_index=True))
```

输出结果：

```
    A   B   C   D
0  A0  B0  C0  D0
1  A1  B1  C1  D1
2  A2  B2  C2  D2
3  A3  B3  C3  D3
4  A4  B4  C4  D1
5  A5  B5  C5  D2
6  A6  B6  C6  D5
7  A7  B7  C7  D6
```

当然 append() 函数也可接收多个对象，示例如下：

```python
a= pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3])
b= pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D1', 'D2', 'D5', 'D6']},
                     index=[4,5,6,7])
c= pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B8', 'B9', 'B10', 'B7'],
                    'C': ['C9', 'C8', 'C7', 'C6'],
                    'D': ['D8', 'D5', 'D7', 'D6']},
                     index=[8,9,10,11])
print(a.append([b,c], ignore_index=True))
```

输出结果：

```
     A    B   C   D
0   A0   B0  C0  D0
1   A1   B1  C1  D1
2   A2   B2  C2  D2
3   A3   B3  C3  D3
4   A4   B4  C4  D1
5   A5   B5  C5  D2
6   A6   B6  C6  D5
7   A7   B7  C7  D6
8   A4   B8  C9  D8
9   A5   B9  C8  D5
10  A6  B10  C7  D7
11  A7   B7  C6  D6
```