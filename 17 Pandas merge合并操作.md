# 17 Pandas merge合并操作

Pandas 提供的 merge() 函数能够进行高效的合并操作，这与 SQL 关系型数据库的 MERGE 用法非常相似。从字面意思上不难理解，merge 翻译为“合并”，指的是将两个 DataFrame 数据表按照指定的规则进行连接，最后拼接成一个新的 DataFrame 数据表。

 merge() 函数的法格式如下：

```python
pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None,left_index=False, right_index=False, sort=True,suffixes=('_x', '_y'), copy=True)
```

参数说明，如下表所示：

| 参数名称    | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| left/right  | 两个不同的 DataFrame 对象。                                  |
| on          | 指定用于连接的键（即列标签的名字），该键必须同时存在于左右两个 DataFrame 中，如果没有指定，并且其他参数也未指定， 那么将会以两个 DataFrame 的列名交集做为连接键。 |
| left_on     | 指定左侧 DataFrame 中作连接键的列名。该参数在左、右列标签名不相同，但表达的含义相同时非常有用。 |
| right_on    | 指定左侧 DataFrame 中作连接键的列名。                        |
| left_index  | 布尔参数，默认为 False。如果为 True 则使用左侧 DataFrame 的行索引作为连接键，若 DataFrame 具有多层 索引(MultiIndex)，则层的数量必须与连接键的数量相等。 |
| right_index | 布尔参数，默认为 False。如果为 True 则使用左侧 DataFrame 的行索引作为连接键。 |
| how         | 要执行的合并类型，从 {'left', 'right', 'outer', 'inner'} 中取值，默认为“inner”内连接。 |
| sort        | 布尔值参数，默认为True，它会将合并后的数据进行排序；若设置为 False，则按照 how 给定的参数值进行排序。 |
| suffixes    | 字符串组成的元组。当左右 DataFrame 存在相同列名时，通过该参数可以在相同的列名后附加后缀名，默认为('_x','_y')。 |
| copy        | 默认为 True，表示对数据进行复制。                            |

注意：Pandas 库的 merge() 支持各种内外连接，与其相似的还有 join() 函数（默认为左连接）。
下面创建两个不同的 DataFrame，然后对它们进行合并操作：

```python
import pandas as pd 
left = pd.DataFrame({ 
   'Company':['Microsoft','Google','Amazon','Apple'], 
   'Name': ['Smith', 'Maiki', 'Hunter', 'Hilen'], 
   'Project_id':['0001','0002','0004','0006']}) 
right = pd.DataFrame({ 
   'Company':['Microsoft','Google','Amazon','Apple'], 
   'Name': ['William', 'Albert', 'Tony', 'Allen'], 
   'Project_id':['0002','0004','0003','0006']}) 
print (left) 
print (right)  
```

输出如下：

```
     Company    Name Project_id
0  Microsoft   Smith       0001
1     Google   Maiki       0002
2     Amazon  Hunter       0004
3      Apple   Hilen       0006

     Company     Name Project_id
0  Microsoft  William       0002
1     Google   Albert       0004
2     Amazon     Tony       0003
3      Apple    Allen       0006
```

#### 1) 在单个键上进行合并操作

通过 on 参数指定一个连接键，然后对上述 DataFrame 进行合并操作：

```python
#通过on参数指定合并的键
print(pd.merge(left,right,on='Project_id'))
```

输出结果：

```
  Company_x  Name_x Project_id  Company_y   Name_y
0    Google   Maiki       0002  Microsoft  William
1    Amazon  Hunter       0004     Google   Albert
2     Apple   Hilen       0006      Apple    Allen
```

#### 2) 在多个键上进行合并操作

下面示例，指定多个键来合并上述两个 DataFrame 对象：

```python
print(pd.merge(left,right,on=['Company','Project_id']))
```

输出结果：

```
  Company Name_x Project_id Name_y
0   Apple  Hilen       0006  Allen
```

## 使用how参数合并

通过`how`参数可以确定 DataFrame 中要包含哪些键，如果在左表、右表都不存的键，那么合并后该键对应的值为 NaN。为了便于大家学习，我们将 how 参数和与其等价的 SQL 语句做了总结：

| Merge方法 | 等效 SQL         | 描述                      |
| --------- | ---------------- | ------------------------- |
| left      | LEFT OUTER JOIN  | 使用左侧对象的key         |
| right     | RIGHT OUTER JOIN | 使用右侧对象的key         |
| outer     | FULL OUTER JOIN  | 使用左右两侧所有key的并集 |
| inner     | INNER JOIN       | 使用左右两侧key的交集     |

#### 1) left join

```python
print(pd.merge(left,right,on='Project_id',how="left"))
```

输出结果：

```
   Company_x  Name_x Project_id  Company_y   Name_y
0  Microsoft   Smith       0001        NaN      NaN
1     Google   Maiki       0002  Microsoft  William
2     Amazon  Hunter       0004     Google   Albert
3      Apple   Hilen       0006      Apple    Allen
```

#### 2) right join

```python
print(pd.merge(left,right,on='Project_id',how="right"))
```

输出结果：

```
  Company_x  Name_x Project_id  Company_y   Name_y
0    Google   Maiki       0002  Microsoft  William
1    Amazon  Hunter       0004     Google   Albert
2       NaN     NaN       0003     Amazon     Tony
3     Apple   Hilen       0006      Apple    Allen
```

#### 3) outer join(并集)

```python
print(pd.merge(left,right,on='Project_id',how="outer"))
```

输出结果：

```
   Company_x  Name_x Project_id  Company_y   Name_y
0  Microsoft   Smith       0001        NaN      NaN
1     Google   Maiki       0002  Microsoft  William
2     Amazon  Hunter       0004     Google   Albert
3      Apple   Hilen       0006      Apple    Allen
4        NaN     NaN       0003     Amazon     Tony
```

#### 4) inner join(交集)

```python
print(pd.merge(left,right,on='Project_id',how="inner"))
```

输出结果：

```
  Company_x  Name_x Project_id  Company_y   Name_y
0    Google   Maiki       0002  Microsoft  William
1    Amazon  Hunter       0004     Google   Albert
2     Apple   Hilen       0006      Apple    Allen
```

注意：当 a 与 b 进行内连操作时 a.join(b) 不等于 b.join(a)。