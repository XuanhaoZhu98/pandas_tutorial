# 25 Pandas分类对象

通常情况下，数据集中会存在许多同一类别的信息，比如相同国家、相同行政编码、相同性别等，当这些相同类别的数据多次出现时，就会给数据处理增添许多麻烦，导致数据集变得臃肿，不能直观、清晰地展示数据。

针对上述问题，Pandas 提供了分类对象（Categorical Object），该对象能够实现有序排列、自动去重的功能。它还可以数字化分类数据，这样我们就可以使用C语言的速度来解决这类文本数据了。本节，我们了解一下分类对象的使用。

## 对象创建

我们可以通过多种方式创建分类对象，下面介绍以下两种方法：

#### 1) 指定dtype创建

```python
import pandas as pd
s = pd.Series(["a","b","c","a"], dtype="category")
print(s)
```

输出结果：

```
0    a
1    b
2    c
3    a
dtype: category
Categories (3, object): [a, b, c]
```

通过上述示例，您可能会注意到，虽然传递给 Series 四个元素值，但是它的类别为 3，这是因为 a 的类别存在重复。

#### 2) 使用astype转换

使用astype将其他类别数据转换为Categories数据，如下所示：

```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice', 'Bob', 'Charlie', 'Danielle'],
                       'balance': [100.0, 200.0, 300.0, 400.0],
                       'gender': ['Female', 'Male', 'Male', 'Female']},
                        columns = ['name', 'balance', 'gender'])
print(df.dtypes)
#改变源数据格式
df['gender'] = df['gender'].astype('category')
print(df.dtypes)
```

输出结果：

```
name        object
balance    float64
gender      object
dtype: object

name         object
balance     float64
gender     category
dtype: object
```

经过转化后，gender数据类型的存储就只有一个字节了。我们也可以继续增加genders的值，这时Pandas将会相应地增加新的值（2,3...）。我们的DataFrame数据外观和感觉跟之前都是一样的，但是此时Pandas内部的结构会使用户的体验更加平滑，这样你就不会感觉到你在使用一个整数数组了

#### 3) pd.Categorical

通过 Category 的构造函数，您可以创建一个类别对象。构造函数，如下所示：

```python
pandas.Categorical(values, categories, ordered)
```

values：以列表的形式传参，表示要分类的值。
ordered：布尔值，默认为 False，若为 Ture，表示对分类的数据进行排序。
dtype：返回一个 category 类型，表示分类对象。

示例如下：

```python
import pandas as pd
#自动按a、b、c分类
cat = pd.Categorical(['a', 'b', 'c', 'a', 'b', 'c'])
print(cat)
```

输出结果：

```
[a, b, c, a, b, c]
Categories (3, object): [a, b, c]
```

再看一组示例：

```python
import pandas as pd
cat=pd.Categorical(['a','b','c','a','b','c','d'], ['c', 'b', 'a'])
print(cat)
```

输出结果：

```
[a, b, c, a, b, c, NaN]
Categories (3, object): [c, b, a]
```

上述示例中，第二个参数值表示类别，当列表中不存在某一类别时，会自动将类别值设置为 NA。

通过指定`ordered=True`来实现有序分类。示例如下：

```python
import pandas as pd
cat=pd.Categorical(['a','b','c','a','b','c','d'], ['c', 'b', 'a'],ordered=True)
print(cat)
#求最小值
print(cat.min())
```

输出结果：

```
['a', 'b', 'c', 'a', 'b', 'c', NaN]
Categories (3, object): ['c' < 'b' < 'a']
c
```

## 获取统计信息

对已经分类的数据使用 describe() 方法，您会得到和数据统计相关的摘要信息。

```python
import pandas as pd
import numpy as np
cat = pd.Categorical(["a", "c", "c", np.nan], categories=["b", "a", "c"])
print(cat)
s = ["a", "c", "c", np.nan]
print(s)
df = pd.DataFrame({"cat":cat, "s": s})
print(df)
print(df.describe())
print(df["cat"].describe())
```

输出结果：

```
['a', 'c', 'c', NaN]
Categories (3, object): ['b', 'a', 'c']
['a', 'c', 'c', nan]

   cat    s
0    a    a
1    c    c
2    c    c
3  NaN  NaN

       cat  s
count    3  3
unique   2  2
top      c  c
freq     2  2

count     3
unique    2
top       c
freq      2
Name: cat, dtype: object
```

## 获取类别属性

使用`obj.categories`命令可以获取对象的类别信息。示例如下：

```python
import pandas as pd
import numpy as np
s = pd.Categorical(["a", "c", "c", np.nan], categories=["b", "a", "c"])
print (s.categories)
```

输出结果：

```
Index(['b', 'a', 'c'], dtype='object')
```

通过 obj.order 可以获取 order 指定的布尔值：

```python
#False表示未指定排序
print (cat.ordered)
```

输出结果：

```
False
```

## 重命名类别

要想对类别实现重命名，可以通过 Series.cat.categories 来实现的，示例如下：

```python
import pandas as pd
s = pd.Series(["a","b","c","a"], dtype="category")
print(s.cat.categories)
#对类名重命名
s.cat.categories = ["Group %s" % g for g in s.cat.categories]
print(s.cat.categories)
```

输出结果：

```
Index(['a', 'b', 'c'], dtype='object')
Index(['Group a', 'Group b', 'Group c'], dtype='object')
```

## 追加新类别

使用 s.cat.add_categories() 方法，可以追加新类别。

```python
import pandas as pd
s = pd.Series(["a","b","c","a"], dtype="category")
print(s.cat.categories)
#追加新类别
s = s.cat.add_categories(['d','e'])
#查看现有类别
print(s.cat.categories)
```

输出结果：

```
Index(['a', 'b', 'c'], dtype='object')
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
```

## 删除类别

使用 remove_categories() 方法，可以删除不需要的类别。示例如下：

```python
import pandas as pd
s = pd.Series(["a","b","c","a"], dtype="category")
#原序列
print(s)
#删除后序列
print(s.cat.remove_categories("a"))
```

输出结果

```
0    a
1    b
2    c
3    a
dtype: category
Categories (3, object): ['a', 'b', 'c']

0    NaN
1      b
2      c
3    NaN
dtype: category
Categories (2, object): ['b', 'c']
```

## 分类对象比较

在下述两种情况下，我们可以对分类对象进行比较：

- 当两个类别对象长度相同时，可以进行比较运算；
- 当两个类别的 ordered 均等于 True，并且类别相同时，可以进行比较运算，比如 ==，！=，>，>=，< 和 <=。

示例如下：

```python
s1=['a','a','b','d','c']
s2=['a','b','b','d','c']
#当满足两个类别长度相同时
ss0 = pd.Categorical(s1)
print(ss0)
ss1 = pd.Categorical(s2)
print(ss1)
print(ss0==ss1)
```

输出结果：

```
['a', 'a', 'b', 'd', 'c']
Categories (4, object): ['a', 'b', 'c', 'd']
['a', 'b', 'b', 'd', 'c']
Categories (4, object): ['a', 'b', 'c', 'd']
[ True False  True  True  True]
```

示例如下：
import pandas as pd

```python
s1=['a','a','b','d','c']
s2=['a','b','b','d','c']
#满足上述第二个条件，类别相同，并且ordered均为True
ss0=pd.Categorical(s1,categories=['a','d','b','c'],ordered=True)
ss1 = pd.Categorical(s2,categories=['a','d','b','c'],ordered=True)
print(ss0<ss1)
```

输出结果：

```
[False  True False False False]
```