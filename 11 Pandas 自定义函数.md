# 11 Pandas自定义函数

如果想要应用自定义的函数，或者把其他库中的函数应用到 Pandas 对象中，有以下三种方法：

- **1)** 操作整个 DataFrame 的函数：pipe()
- **2)** 操作行或者列的函数：apply()
- **3)** 操作单一元素的函数：applymap()


如何从上述函数中选择适合的函数，这取决于函数的操作对象。下面介绍了三种方法的使用。

## 操作整个数据表 pipe()

通过给 pipe() 函数传递一个自定义函数和适当数量的参数值，从而操作 DataFrme 中的所有元素。下面示例，实现了数据表中的元素值依次加 3。

首先自定义一个函数，计算两个元素的加和，如下所示：

```python
def adder(ele1,ele2):
	return ele1+ele2
```

然后使用自定义的函数对 DataFrame 进行操作：

```python
df = pd.DataFrame(np.random.randn(4,3),columns=['c1','c2','c3'])
#传入自定义函数以及要相加的数值3
df.pipe(adder,3)
```

完整的程序，如下所示：

```nmd 6
import pandas as pd
import numpy as np
#自定义函数
def adder(ele1,ele2):
   return ele1+ele2
#操作DataFrame
df = pd.DataFrame(np.random.randn(4,3),columns=['c1','c2','c3'])
#相加前
print(df)
#相加后
print(df.pipe(adder,3))
```

输出结果：

```
         c1        c2        c3
0  1.989075  0.932426 -0.523568
1 -1.736317  0.703575 -0.819940
2  0.657279 -0.872929  0.040841
3  0.441424  1.170723 -0.629618
         c1        c2        c3
0  4.989075  3.932426  2.476432
1  1.263683  3.703575  2.180060
2  3.657279  2.127071  3.040841
3  3.441424  4.170723  2.370382
```

## 操作行或列 apply()

如果要操作 DataFrame 的某一行或者某一列，可以使用 apply() 方法，该方法与描述性统计方法类似，都有可选参数 axis，并且**默认按列操作**。示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
df.apply(np.mean)
#默认按列操作，计算每一列均值
print(df.apply(np.mean))
```

输出结果：

```
col1    0.277214
col2    0.716651
col3   -0.250487
dtype: float64
```

传递轴参 axis=1， 表示逐行进行操作，示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
print(df)
print (df.apply(np.mean,axis=1))
```

输出结果：

```
       col1      col2      col3
0  0.210370 -0.662840 -0.281454
1 -0.875735  0.531935 -0.283924
2  1.036009 -0.958771 -1.048961
3 -1.266042 -0.257666  0.403416
4  0.496041 -1.071545  1.432817

0   -0.244641
1   -0.209242
2   -0.323908
3   -0.373431
4    0.285771
dtype: float64
```

求每一列中，最大值与最小值之差。示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
print(df.apply(lambda x: x.max() - x.min()))
```

输出结果:

```
col1    2.221391
col2    1.809087
col3    3.639235
dtype: float64
```

同样的，也可以对行进行类似操作

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
print(df.apply(lambda x: x.max() - x.min(), axis = 1))
```

输出结果:

```
0    2.325186
1    2.078723
2    1.021062
3    1.471965
4    3.535440
dtype: float64
```



## 操作单一元素 applymap()

DataFrame 数据表结构的 applymap() 和 Series 系列结构的 map() 类似，它们都可以接受一个 Python 函数，并返回相应的值。

示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
#自定义函数lambda函数
print(df['col1'].map(lambda x:x*100))
```

输出结果：

```
0    -18.171706
1      1.582861
2     22.398156
3     32.395690
4   -133.143543
Name: col1, dtype: float64
```

下面示例使用了 applymap() 函数，如下所示：

```python
import pandas as pd
import numpy as np
#自定义函数
df = pd.DataFrame(np.random.randn(5,3),columns=['col1','col2','col3'])
print(df)
print(df.applymap(lambda x:x*10))
```

输出结果：

```
       col1      col2      col3
0  2.996455  0.318589 -0.016571
1  0.316849  1.012609  1.283812
2 -0.118513  0.367123 -0.682451
3 -0.287845 -0.179503  2.232839
4  0.004543  0.110816 -0.538806

        col1       col2       col3
0  29.964551   3.185889  -0.165713
1   3.168486  10.126093  12.838119
2  -1.185126   3.671229  -6.824514
3  -2.878450  -1.795031  22.328389
4   0.045429   1.108158  -5.388061

```