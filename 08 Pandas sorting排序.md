# 08 Pandas sorting排序

Pands 提供了两种排序方法，分别是按标签排序和按数值排序。本节讲解 Pandas 的排序操作。

下面创建一组 DataFrame 数据，如下所示：

```python
import pandas as pd
import numpy as np
#行标签乱序排列，列标签乱序排列
unsorted_df=pd.DataFrame(np.random.randn(10,2),index=[1,6,4,2,3,5,9,8,0,7],columns=['col2','col1'])
print(unsorted_df)
```

输出结果：

```
       col2      col1
1 -0.053290 -1.442997
6 -0.203066 -0.702727
4  0.111759  0.965251
2 -0.896778  1.100156
3 -0.458899 -0.890152
5 -0.222691 -0.144881
9 -0.921674  0.510045
8 -0.130748 -0.734237
0  0.617717  0.456848
7  0.804284  0.653961
```

上述示例，行标签和数值元素均未排序，下面分别使用标签排序、数值排序对其进行操作。

## 按标签排序

使用 sort_index() 方法对行标签排序，指定轴参数（axis）或者排序顺序。或者可以对 DataFrame 进行排序。默认情况下，按照行标签序排序。

```python
import pandas as pd
import numpy as np
unsorted_df = pd.DataFrame(np.random.randn(10,2),index=[1,4,6,2,3,5,9,8,0,7],columns = ['col2','col1'])
sorted_df=unsorted_df.sort_index()
print(sorted_df)
```

输出结果：

```
       col2      col1
0  2.113698 -0.299936
1 -0.550613  0.501497
2  0.056210  0.451781
3  0.074262 -1.249118
4 -0.038484 -0.078351
5  0.812215 -0.757685
6  0.687233 -0.356840
7 -0.483742  0.632428
8 -1.576988 -1.425604
9  0.776720  1.182877
```

#### 1) 排序顺序

通过将布尔值传递给`ascending`参数，可以控制排序的顺序（行号顺序）。示例如下：

```python
import pandas as pd
import numpy as np
unsorted_df = pd.DataFrame(np.random.randn(10,2),index=[1,4,6,2,3,5,9,8,0,7],columns = ['col2','col1'])
sorted_df = unsorted_df.sort_index(ascending=False)
print(sorted_df)
```

输出结果：

```
       col2      col1
9  2.389933  1.152328
8 -0.374969  0.182293
7 -0.823322 -0.104431
6 -0.566627 -1.020679
5  1.021873  0.315927
4  0.127070 -1.598591
3  0.258097  0.389310
2 -1.027768 -0.582664
1  0.766471 -0.043638
0  0.482486 -0.512309
```

## 按列标签排序

通过给 axis 轴参数传递 0 或 1，可以对列标签进行排序。默认情况下，axis=0 表示按行排序；而 axis=1 则表示按列排序。

```python
import pandas as pd
import numpy as np
unsorted_df = pd.DataFrame(np.random.randn(10,2),index=[1,4,6,2,3,5,9,8,0,7],columns = ['col2','col1'])
sorted_df=unsorted_df.sort_index(axis=1)
print (sorted_df)
```

输出结果：

```
       col1      col2
1 -1.424992 -0.062026
4 -0.083513  1.884481
6 -1.335838  0.838729
2 -0.085384  0.178404
3  1.198965  0.089953
5  1.400264  0.213751
9 -0.992759  0.015740
8  1.586437 -0.406583
0 -0.842969  0.490832
7 -0.310137  0.485835
```

## 按值排序

与标签排序类似，sort_values() 表示按值排序。它接受一个`by`参数，该参数值是要排序数列的 DataFrame 列名。示例如下：

```python
import pandas as pd
import numpy as np
unsorted_df = pd.DataFrame({'col1':[2,1,1,1],'col2':[1,3,2,4]})
sorted_df = unsorted_df.sort_values(by='col1')
print (sorted_df)
```

输出结果：

```
   col1  col2
1     1     3
2     1     2
3     1     4
0     2     1
```

注意：by 参数可以接受一个列表参数值，按照多行进行排序，如下所示：

```python
import pandas as pd
import numpy as np
unsorted_df = pd.DataFrame({'col1':[2,1,1,1],'col2':[1,3,2,4]})
sorted_df = unsorted_df.sort_values(by=['col1','col2'])
print (sorted_df）
```

输出结果：

```
   col1  col2
2     1     2
1     1     3
3     1     4
0     2     1
```

## 排序算法

sort_values() 提供了参数`kind`用来指定排序算法。这里有三种排序算法：

- mergesort
- heapsort
- quicksort


默认为 quicksort(快速排序) ，其中 Mergesort 归并排序是最稳定的算法。

```python
import pandas as pd
import numpy as np
unsorted_df = pd.DataFrame({'col1':[2,1,1,1],'col2':[1,3,2,4]})
sorted_df = unsorted_df.sort_values(by='col1' ,kind='mergesort')
print (sorted_df)
```

输出结果：

```
   col1  col2
1     1     3
2     1     2
3     1     4
0     2     1
```