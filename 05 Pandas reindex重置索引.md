# 05 Pandas reindex重置索引

重置索引（reindex）可以更改原 DataFrame 的行标签或列标签，并使更改后的行、列标签与 DataFrame 中的数据逐一匹配。通过重置索引操作，您可以完成对现有数据的重新排序。如果重置的索引标签在原 DataFrame 中不存在，那么该标签对应的元素值将全部填充为 NaN。

## 重置行列标签

看一组简单示例：

```python
import pandas as pd
import numpy as np
N=20
df = pd.DataFrame({
   'A': pd.date_range(start='2022-01-01',periods=N,freq='D'),
   'x': np.linspace(0,stop=N-1,num=N),
   'y': np.random.rand(N),
   'C': np.random.choice(['Low','Medium','High'],N).tolist(),
   'D': np.random.normal(100, 10, size=(N)).tolist()
})
print(df)
#重置行、列索引标签
df_reindexed = df.reindex(index=[0,2,5], columns=['A', 'C', 'B'])
print(df_reindexed)
```

输出结果：

```
            A     x         y       C           D
0  2016-01-01   0.0  0.100799     Low   97.403759
1  2016-01-02   1.0  0.735440     Low  112.155553
2  2016-01-03   2.0  0.842693    High   87.393686
3  2016-01-04   3.0  0.734467    High   97.186882
4  2016-01-05   4.0  0.244086     Low   85.530586
5  2016-01-06   5.0  0.393671  Medium   99.706310
6  2016-01-07   6.0  0.790591  Medium  103.382289
7  2016-01-08   7.0  0.709151  Medium  111.383196
8  2016-01-09   8.0  0.035173     Low   93.560543
9  2016-01-10   9.0  0.412323     Low   99.643911
10 2016-01-11  10.0  0.722114    High   92.332991
11 2016-01-12  11.0  0.798983  Medium  103.001798
12 2016-01-13  12.0  0.815537     Low   88.230087
13 2016-01-14  13.0  0.372957     Low   90.234982
14 2016-01-15  14.0  0.951258  Medium  102.613723
15 2016-01-16  15.0  0.633969  Medium  106.217799
16 2016-01-17  16.0  0.179276  Medium  114.876282
17 2016-01-18  17.0  0.302991     Low   91.191671
18 2016-01-19  18.0  0.240571    High   80.285203
19 2016-01-20  19.0  0.244529    High   95.936290

           A       C   B
0 2016-01-01     Low NaN
2 2016-01-03    High NaN
5 2016-01-06  Medium NaN
```

填充空缺值：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame({ 'A': ['Tom','Jim','May'],'B': [1,2,3]},index=[0,2,4])
print(df)
df.reindex(index=[0,1,2,3,4])
print(df)
df.reindex(index=[0,1,2,3,4],method='ffill')
print(df)
df.reindex(index=[0,1,2,3,4],method='bfill')
print(df)
```

输出结果：

```
     A  B
0  Tom  1
2  Jim  2
4  May  3

     A    B
0  Tom  1.0
1  NaN  NaN
2  Jim  2.0
3  NaN  NaN
4  May  3.0

     A  B
0  Tom  1
1  Tom  1
2  Jim  2
3  Jim  2
4  May  3

     A  B
0  Tom  1
1  Jim  2
2  Jim  2
3  May  3
4  May  3
```

重置 DataFrame 的索引，并使用默认索引

```python
import pandas as pd
import numpy as np
df = pd.DataFrame({ 'A': ['Tom','Jim','May'],'B': [1,2,3]},index=[0,2,4])
print(df)
df = df.reset_index(drop=True)
print(df)
```

输出结果：

```
     A  B
0  Tom  1
2  Jim  2
4  May  3

     A  B
0  Tom  1
1  Jim  2
2  May  3
```

现有 a、b 两个 DataFrame 对象，如果想让 a 的行索引与 b 相同，您可以使用 reindex_like() 方法。示例如下：

```python
import pandas as pd
import numpy as np
a = pd.DataFrame(np.random.randn(10,3),columns=['col1','col2','col3'])
b = pd.DataFrame(np.random.randn(7,3),columns=['col1','col2','col3'])
a= a.reindex_like(b)
print(a)
```

输出结果：

```
       col1      col2      col3
0  1.776556 -0.821724 -1.220195
1 -1.401443  0.317407 -0.663848
2  0.300353 -1.010991  0.939143
3  0.444041 -1.875384  0.846112
4  0.967159  0.369450 -0.414128
5  0.320863 -1.223477 -0.337110
6 -0.933665  0.909382  1.129481
```

上述示例，a 会按照 b 的形式重建行索引。需要特别注意的是，a 与 b 的列索引标签必须相同。

## 填充元素值

reindex_like() 提供了一个可选的参数`method`，使用它来填充相应的元素值，参数值介绍如下：

- pad/ffill：从前向后填充值；
- bfill/backfill：从后向前填充值；
- nearest：从距离最近的索引值开始填充。


示例如下：

```python
import pandas as pd
import numpy as np

df1 = pd.DataFrame(np.random.randn(6,3),columns=['col1','col2','col3'])
df2 = pd.DataFrame(np.random.randn(2,3),columns=['col1','col2','col3'])
#使df2和df1行标签相同
print(df2.reindex_like(df1))
#向前填充
print(df2.reindex_like(df1,method='ffill'))
```

输出结果：

```
#填充前
       col1      col2      col3
0  0.129055  0.835440  0.383065
1 -0.357231  0.379293  1.211549
2       NaN       NaN       NaN
3       NaN       NaN       NaN
4       NaN       NaN       NaN
5       NaN       NaN       NaN
#填充后
       col1      col2      col3
0  0.129055  0.835440  0.383065
1 -0.357231  0.379293  1.211549
2 -0.357231  0.379293  1.211549
3 -0.357231  0.379293  1.211549
4 -0.357231  0.379293  1.211549
5 -0.357231  0.379293  1.211549
```

## 限制填充行数

reindex_like() 还提供了一个额外参数 limit，该参数用来控制填充的最大行数。示例如下：

```python
import pandas as pd
import numpy as np
df1 = pd.DataFrame(np.random.randn(6,3),columns=['col1','col2','col3'])
df2 = pd.DataFrame(np.random.randn(2,3),columns=['col1','col2','col3'])
print (df2.reindex_like(df1))
#最多填充2行
print (df2.reindex_like(df1,method='ffill',limit=2))
```

输出结果：

```
       col1      col2      col3
0 -1.829469  0.310332 -2.008861
1 -1.038512  0.749333 -0.094335
2       NaN       NaN       NaN
3       NaN       NaN       NaN
4       NaN       NaN       NaN
5       NaN       NaN       NaN

       col1      col2      col3
0 -1.829469  0.310332 -2.008861
1 -1.038512  0.749333 -0.094335
2 -1.038512  0.749333 -0.094335
3 -1.038512  0.749333 -0.094335
4       NaN       NaN       NaN
5       NaN       NaN       NaN
```

由上述示例可以看出，填充了 2、3 行 缺失值，也就是只填充了 2 行数据。

## 现有列设置索引

使用一个或多个现有列或数组（长度正确）设置 DataFrame 索引（行标签）。索引可以替换现有索引或对其进行扩展，示例如下：

```python
df = pd.DataFrame({'month': [1, 4, 7, 10], 'year': [2012, 2014, 2013, 2014], 'sale': [55, 40, 84, 31]})
print(df)
#将索引设置为“month”列
df = df.set_index('month')
print(df)
```

输出结果：

```
   month  year  sale
0      1  2012    55
1      4  2014    40
2      7  2013    84
3     10  2014    31

       year  sale
month            
1      2012    55
4      2014    40
7      2013    84
10     2014    31
```

使用列 'year' 和 'month' 创建一个 MultiIndex：

```python
df = df.set_index(['year', 'month'])
print(df)
```

输出结果：

```
            sale
year month      
2012 1        55
2014 4        40
2013 7        84
2014 10       31
```

使用索引和列创建 MultiIndex：

```python
df = df.set_index([pd.Index([1, 2, 3, 4]), 'year'])
print(df)
```

输出结果：

```
        month  sale
  year             
1 2012      1    55
2 2014      4    40
3 2013      7    84
4 2014     10    31
```

使用两个系列创建一个 MultiIndex：

```python
s = pd.Series([1, 2, 3, 4])
df = df.set_index([s, s**2])
print(df)
```

输出结果：

```
      month  year  sale
1 1       1  2012    55
2 4       4  2014    40
3 9       7  2013    84
4 16     10  2014    31
```

## 重命名标签

rename() 方法允许您使用某些映射(dict或Series)或任意函数来对行、列标签重新命名，示例如下：

```python
import pandas as pd
import numpy as np
df1 = pd.DataFrame(np.random.randn(6,3),columns=['col1','col2','col3'])
print (df1)
#对行和列重新命名
print (df1.rename(columns={'col1' : 'c1', 'col2' : 'c2'},index = {0 : 'apple', 1 : 'banana', 2 : 'durian'}))
```

输出结果：

```
       col1      col2      col3
0 -1.762133 -0.636819 -0.309572
1 -0.093965 -0.924387 -2.031457
2 -1.231485 -0.738667  1.415724
3 -0.826322  0.206574 -0.731701
4  1.863816 -0.175705  0.491907
5  0.677361  0.870041 -0.636518

              c1        c2      col3
apple  -1.762133 -0.636819 -0.309572
banana -0.093965 -0.924387 -2.031457
durian -1.231485 -0.738667  1.415724
3      -0.826322  0.206574 -0.731701
4       1.863816 -0.175705  0.491907
5       0.677361  0.870041 -0.636518
```

rename() 方法提供了一个 inplace 参数，默认值为 False，表示拷贝一份原数据，并在复制后的数据上做重命名操作。若 inplace=True 则表示在原数据的基础上重命名。