# 27 Pandas和NumPy的比较

## Pandans与NumPy区别

Pandas 和 NumPy 被认为是科学计算与机器学习中必不可少的库，因为它们具有直观的语法和高性能的矩阵计算能力。下面对 Pandas 与 NumPy 进行简单的总结，如下表所示：

| 比较项     | Pandas                                   | NumPy                                          |
| ---------- | ---------------------------------------- | ---------------------------------------------- |
| 适应性     | Pandas主要用来处理类表格数据。           | NumPy 主要用来处理数值数据。                   |
| 工具       | Pandas提供了Series和DataFrame数据结构。  | NumPy 构建了 ndarray array来容纳数据。         |
| 性能       | Pandas对于处理50万行以上的数据更具优势。 | NumPy 则对于50万以下或者更少的数据，性能更佳。 |
| 内存利用率 | 与 NumPy相比，Pandas会消耗大量的内存。   | NumPy 会消耗较少的内存。                       |
| 对象       | Pandas 提供了 DataFrame 2D数据表对象。   | NumPy 则提供了一个多维数组 ndarray 对象        |

纵然NumPy与Pandas风靡于数据分析任务，人们对其的不足也多有指出，其中最主要的便是由于Python自身的动态语言特性而带来的运行速度方面的损失，其次便是Python在大数据处理方面（数G甚至几十上百G）的捉襟见肘。

对于运算速度方面的损失，NumPy已经做了相当程度的优化，可以对大数组的数据进行高效处理。优化包括

- NumPy是在一个连续的内存块中存储数据，独立于其他Python内置对象，如此便可以加速数据索引的速度。
- 其次，NumPy调用了大量的用C语言编写的算法库，使得其可以直接操作内存，不必进行Python动态语言特性所含有的前期类型检查工作，从而大大提高了运算速度。
- 最后，NumPy所有独有的可以在整个数组上执行复杂的计算也能够大幅提高运算效率（基于NumPy的算法要比纯Python快10到100倍，甚至会快更多）。

而对于大数据的处理，经过合理的优化，Python处理几个G的数据绰绰有余，至于几十G也勉强可以，而上百G的数据这就算是Hadoop与Spark系列的任务，不是Python的NumPy与pandas可以应付的，也不是R语言某个第三方包可以处理的。

NumPy除了在相当程度上优化了Python计算过程，其自身还有较多的高级特性，如指定数组存储的行优先或者列优先、广播功能从而快速的对不同形状的矩阵进行计算、ufunc类型的函数可以使得我们丢开循环而编写出更为简洁也更有效率的代码、使用开源项目Numba编写快速的NumPy函数，而Numba则是可以利用GPU进行运算的。

虽然NumPy有着以上的种种出色的特性，其本身则难以独支数据分析这座大厦，这是一方面是由于NumPy几乎仅专注于数组处理，另一方面则是数据分析牵涉到的数据特性众多，需要处理各种表格和混杂数据，远非纯粹的数组（NumPy）方便解决的，而这就是pandas发力的地方。

pandas 这个名称来源于panel data（面板数据），从而可见其要处理的数据是多维度的而非单维度。pandas 含有使数据清洗和分析工作变得更快更简单的数据结构与操作工具。经常是和其他工具一起使用，如数值计算工具NumPy和SciPy，分析库statsmodels与scikit-learn，以及数据可视化库matplotlib。其中NumPy则是构建pandas的基础，后者大量借鉴了NumPy编码风格。

如果用做一餐饭来比喻，pandas于处理数据方面的功用则相当于将米洗好，将菜摘好洗好以及切好的过程，至于入锅添油加醋，锅铲捣腾，做成一道菜则是依靠statsmodels、scikit-learn和matplotlib的功能。

pandas功能特性广泛，其包含的函数类型也众多，数据结构有Series与DataFrame，函数类型有索引函数、汇总函数、加载以及保存众多文件格式函数、与数据库交互函数、字符串处理函数、缺失数据处理函数、合并重塑轴向旋转表格型数据函数、简单的绘图函数、数据聚合（groupby）分组运算（apply）函数、透视表交叉表函数以及时间序列处理方面的各种函数。

## 数据结构差异

### NumPy:N维数组容器

Numpy是用python进行科学计算的一个基础库，因为它提供了python基础包所没有提供的数据结构和高性能函数。Numpy定义了一种专门用于科学计算的数据结构`ndarray` – 它是一种N维数组。主要特点如下:

- 内存块风格
  这是因为ndarray中的所有元素的类型都是相同的，而Python列表中的元素类型是任意的，所以ndarray在存储元素时内存可以连续，而python原生list就只能通过寻址方式找到下一个元素，这虽然也导致了在通用性能方面Numpy的ndarray不及Python原生list，但在科学计算中，Numpy的ndarray就可以省掉很多循环语句，代码使用方面比Python原生list简单的多。
- ndarray支持并行化运算(向量化运算)
- Numpy底层使用C语言编写，内部解除了GIL（全局解释器锁），其对数组的操作速度不受Python解释器的限制，效率远高于纯Python代码。

数组的创建方法有几种，最常用的就是使用array()函数，参数为单层或者嵌套列表，如下所示：

```python
import numpy as np
e = np.array([[1,2,3],[4,5,6],[7,8,9]])
print(e)
```

输出结果：

```
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```

### Pandas:表格容器

[Pandas](http://geek-docs.com/pandas) 的核心为两种数据结构，数据分析相关的所有事务都是围绕着这两种数据结构进行的：`Series`和`DataFrame`。Pandas库的Series对象用来表示一维数据结构，跟数组类似。

```python
import pandas as pd
ser = pd.Series([4,7,5,3])
print(ser)
```

输出结果：

```
0    4
1    7
2    5
3    3
dtype: int64
```

如上所示：0，1，2，3是索引，4，7，5，3是值，其中索引是可以指定的

DataFrame这种列表式数据结构跟工作表(最常见的Excel工作表)极为相似，其设计初衷是将Series的使用场景由一维扩展到多维。DataFrame是按一定顺序排列的多列数据组成，各列的数据类型可以有所不同（数值、字符串或布尔值等）。如下所示：

```python
import pandas as pd
data = {'color':['blue', 'green', 'yellow', 'red', 'white'],
        'object':['ball', 'pan', 'pencil', 'paper', 'mug'],
        'price':[1.2, 1.0, 0.6, 0.9, 1.7]}
frame = pd.DataFrame(data)
print(frame)
```

输出结果：

```
    color  object  price
0    blue    ball    1.2
1   green     pan    1.0
2  yellow  pencil    0.6
3     red   paper    0.9
4   white     mug    1.7
```

pandas可以读取较多类型的文件格式，从简单的txt、csv、json到excel，hdf5、pickle再到sas、sql、stata等等文件格式都有得以支持。在读取数据时，函数会使用到若干技术将数据转换成DataFrame格式，如索引、类型推断和数据转换、日期解析、迭代与不规整数据问题等。

至于pandas与数据库交互，它可以通过特定的第三方包实现将SQL Server、 PostgreSQL和MySQL数据库中的数据加载到DataFrame中，然后进行各种处理分析。

pandas自带的绘图函数较为简陋，只有简单的plot函数，不过Series或者DataFrame格式的数据可以与matplotlib以及seaborn等绘图工具结合以绘制各类精致的图例。

最后分享的是pandas的时间序列类函数，可以说是pandas让处理时间序列数据变得得心应手。第三方包datetime与dateutil能够将识别与处理多种时间格式，pandas自身可以生成指定频率的DatetimeIndex，也可以处理时区信息。其移动窗口函数则是大大方便了时间序列分析，使得建立各种AR、MA、ARMA、ARIMA等等时间序列模型方便快捷，而这正是R语言的领地。

Python因为有了NumPy与pandas而不同于Java、C#等程序语言，Python也因为NumPy与pandas而又一次的焕发了光彩。NumPy与pandas也许多少借鉴了R语言的特性与功能，但它也的确做的了在某些方面青出于蓝胜于蓝。

## 转换ndarray数组

在某些情况下，需要执行一些 NumPy 数值计算的高级函数，这个时候您可以使用 to_numpy() 函数，将 DataFrame 对象转换为 NumPy ndarray 数组，并将其返回。函数的语法格式如下：

DataFrame.to_numpy(dtype=None, copy=False)  

参数说明如下：

- dtype：可选参数，表示数据类型；
- copy：布尔值参数，默认值为 Fales，表示返回值不是其他数组的视图。


下面使用示例，了解该函数的使用方法。示例：

```python
info = pd.DataFrame({"P": [2, 3], "Q": [4.0, 5.8]})
#给info添加R列 
info['R'] = pd.date_range('2022-12-23', periods=2)
print(info)
#将其转化为numpy数组 
n=info.to_numpy()
print(n)
#通过 type 查看其类型
print(type(n))
```

输出结果：

```
   P    Q          R
0  2  4.0 2022-12-23
1  3  5.8 2022-12-24

[[2 4.0 Timestamp('2022-12-23 00:00:00')]
 [3 5.8 Timestamp('2022-12-24 00:00:00')]]
 
<class 'numpy.ndarray'>
```