# 15 Pandas窗口函数

为了能更好地处理数值型数据，Pandas 提供了几种窗口函数，比如移动函数（rolling）、扩展函数（expanding）和指数加权函数（ewm）。

窗口函数应用场景非常多。举一个简单的例子：现在有 10 天的销售额，而您想每 3 天求一次销售总和，也就说第五天的销售额等于（第三天 + 第四天 + 第五天）的销售额之和，此时窗口函数就派上用场了。

窗口是一种形象化的叫法，这些函数在执行操作时，就如同窗口一样在数据区间上移动。

本节学习主要讲解如何在 DataFrame 和 Series 对象上应用窗口函数。

## 移动窗口函数 rolling()

rolling() 又称移动窗口函数，它可以与 mean、count、sum、median、std 等聚合函数一起使用。为了使用方便，Pandas 为移动函数定义了专门的方法聚合方法，比如 rolling_mean()、rolling_count()、rolling_sum() 等。其的语法格式如下：

```python
rolling(window=n, min_periods=None, center=False)
```

常用参数说明如下：

| 参数名称    | 说明                                                       |
| ----------- | ---------------------------------------------------------- |
| window      | 默认值为 1，表示窗口的大小，也就是观测值的数量，           |
| min_periods | 表示窗口的最小观察值，默认与 window 的参数值相等。         |
| center      | 是否把中间值做为窗口标准，默认值为 False。
下面看一组示例： |

```python
import pandas as pd
import numpy as np
#生成时间序列
df = pd.DataFrame(np.random.randn(8, 4),index = pd.date_range('12/1/2020', periods=8),columns = ['A', 'B', 'C', 'D'])
print(df)
#每3个数求求一次均值
print(df.rolling(window=3).mean())
#每3个数求求一次和
print(df.rolling(window=3).sum())
```

输出结果：

```
                   A         B         C         D
2020-12-01  2.001150  0.264148 -1.026360 -0.614928
2020-12-02  0.098606  0.120017 -0.270654 -1.070103
2020-12-03 -1.272756  0.454024  0.524804  0.695915
2020-12-04 -0.406008  0.220658  0.729193  0.962950
2020-12-05 -0.043184  0.647777 -1.321048 -0.442746
2020-12-06  1.486957 -0.252886  0.789525 -0.994056
2020-12-07 -0.965410  0.737693  1.295197 -0.215026
2020-12-08  0.721626 -0.259525 -2.642019 -0.055333

                   A         B         C         D
2020-12-01       NaN       NaN       NaN       NaN
2020-12-02       NaN       NaN       NaN       NaN
2020-12-03  0.275667  0.279396 -0.257404 -0.329705
2020-12-04 -0.526719  0.264900  0.327781  0.196254
2020-12-05 -0.573983  0.440820 -0.022350  0.405373
2020-12-06  0.345922  0.205183  0.065890 -0.157951
2020-12-07  0.159454  0.377528  0.254558 -0.550609
2020-12-08  0.414391  0.075094 -0.185766 -0.421472

                   A         B         C         D
2020-12-01       NaN       NaN       NaN       NaN
2020-12-02       NaN       NaN       NaN       NaN
2020-12-03  0.827000  0.838189 -0.772211 -0.989116
2020-12-04 -1.580158  0.794700  0.983343  0.588762
2020-12-05 -1.721948  1.322459 -0.067051  1.216119
2020-12-06  1.037765  0.615549  0.197670 -0.473852
2020-12-07  0.478363  1.132584  0.763674 -1.651828
2020-12-08  1.243173  0.225282 -0.557297 -1.264415

```

`window=3`表示是每一列中依次紧邻的每 3 个数求一次均值。当不满足 3 个数时，所求值均为 NaN 值，因此前两列的值为 NaN，直到第三行值才满足要求 window =3。求均值的公式如下所示：

```
(index1+index2+index3)/3
```

rolling()函数除了mean()，还支持很多函数，比如：

- count() 非空观测值数量
- sum() 值的总和
- median() 值的算术中值
- min() 最小值
- max() 最大
- std() 贝塞尔修正样本标准差
- var() 无偏方差
- skew() 样品偏斜度（三阶矩）
- kurt() 样品峰度（四阶矩）
- quantile() 样本分位数（百分位上的值）
- cov() 无偏协方差（二元）
- corr() 相关（二进制）

## 扩展窗口函数 expanding()

expanding() 又叫扩展窗口函数，扩展是指由序列的第一个元素开始，逐个向后计算元素的聚合值。

rolling()函数，是固定窗口大小，进行滑动计算，expanding()函数只设置最小的观测值数量，不固定窗口大小，实现累计计算，即不断扩展

expanding()函数，类似cumsum()函数的累计求和，其优势在于还可以进行更多的聚类计算

下面示例，`min_periods = n`表示向后移动 n 个值计求一次平均值：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(10, 4),
      index = pd.date_range('1/1/2022', periods=10),
      columns = ['A', 'B', 'C', 'D'])
print (df.expanding(min_periods=3).mean())
print (df.expanding(min_periods=3).sum())
#比较cumsum()和expanding()
print(df.cumsum())
print(df.expanding(min_periods=1).sum())
```

输出结果：

```
                   A         B         C         D
2022-01-01  1.011641  0.457647  1.552729  0.596952
2022-01-02 -0.476765  0.753504  0.523117  0.743132
2022-01-03 -0.212496  1.438729  0.819701  0.583567
2022-01-04  2.765715 -0.716846 -1.228101 -1.143268
2022-01-05  0.509428  1.804440 -0.734525  1.586750
2022-01-06  0.287999  1.650563  1.073510 -0.767504
2022-01-07 -0.770709 -0.359967  0.351277  0.194904
2022-01-08 -0.663622  0.021462 -0.351861  0.824238
2022-01-09 -1.088861 -0.020123  0.906767 -2.085902
2022-01-10 -0.889099 -0.980421  0.317468  0.107012

                   A         B         C         D
2022-01-01       NaN       NaN       NaN       NaN
2022-01-02       NaN       NaN       NaN       NaN
2022-01-03  0.107460  0.883293  0.965182  0.641217
2022-01-04  0.772024  0.483259  0.416862  0.195096
2022-01-05  0.719505  0.747495  0.186584  0.473427
2022-01-06  0.647587  0.898006  0.334405  0.266605
2022-01-07  0.444973  0.718296  0.336816  0.256362
2022-01-08  0.306399  0.631191  0.250731  0.327346
2022-01-09  0.151370  0.558823  0.323624  0.059208
2022-01-10  0.047323  0.404899  0.323008  0.063988

                   A         B         C         D
2022-01-01       NaN       NaN       NaN       NaN
2022-01-02       NaN       NaN       NaN       NaN
2022-01-03  0.322380  2.649880  2.895547  1.923651
2022-01-04  3.088095  1.933034  1.667446  0.780383
2022-01-05  3.597523  3.737474  0.932922  2.367133
2022-01-06  3.885522  5.388037  2.006431  1.599629
2022-01-07  3.114813  5.028070  2.357709  1.794533
2022-01-08  2.451191  5.049532  2.005848  2.618771
2022-01-09  1.362330  5.029409  2.912615  0.532869
2022-01-10  0.473231  4.048988  3.230082  0.639881

                   A         B         C         D
2022-01-01  1.011641  0.457647  1.552729  0.596952
2022-01-02  0.534876  1.211151  2.075846  1.340084
2022-01-03  0.322380  2.649880  2.895547  1.923651
2022-01-04  3.088095  1.933034  1.667446  0.780383
2022-01-05  3.597523  3.737474  0.932922  2.367133
2022-01-06  3.885522  5.388037  2.006431  1.599629
2022-01-07  3.114813  5.028070  2.357709  1.794533
2022-01-08  2.451191  5.049532  2.005848  2.618771
2022-01-09  1.362330  5.029409  2.912615  0.532869
2022-01-10  0.473231  4.048988  3.230082  0.639881

                   A         B         C         D
2022-01-01  1.011641  0.457647  1.552729  0.596952
2022-01-02  0.534876  1.211151  2.075846  1.340084
2022-01-03  0.322380  2.649880  2.895547  1.923651
2022-01-04  3.088095  1.933034  1.667446  0.780383
2022-01-05  3.597523  3.737474  0.932922  2.367133
2022-01-06  3.885522  5.388037  2.006431  1.599629
2022-01-07  3.114813  5.028070  2.357709  1.794533
2022-01-08  2.451191  5.049532  2.005848  2.618771
2022-01-09  1.362330  5.029409  2.912615  0.532869
2022-01-10  0.473231  4.048988  3.230082  0.639881
```

设置 min_periods=3，表示至少 3 个数求一次均值，计算方式为 (index0+index1+index2)/3，而 index3 的计算方式是 (index0+index1+index2+index3)/4，依次类推。

## 指数加权移动 ewm()

ewm（全称 Exponentially Weighted Moving）表示指数加权移动。ewn() 函数先会对序列元素做指数加权运算，其次计算加权后的均值。该函数通过指定 com、span 或者 halflife 参数来实现指数加权移动，必须提供质心(com)、跨度(span)、半衰期(half-life)和alpha值之一。

| 参数名称 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| com      | 浮点数，可选根据质心指定衰减，α = 1/(1+com)，对于com>=0      |
| span     | 浮点数，可选根据跨度指定衰减，α = 1/(1+span)，对于span>=0    |
| halflife | 浮点数，指定衰减 half-life，α = 1-exp(-ln(2)=halflife)，对于halflife>=0 |
| alpha    | 浮点数，直接指定平滑因子，0<α<=1                             |

ewm对最近的观察结果给予更多的权重，或者随着数据被及时删除，它对数据的权重越来越小，因此它可以更快地捕捉最近的趋势。

在ewm中，应用的权重取决于“adjust parameters”和number of periods。adjust默认为真。

使用公式计算重量：

![img](https://miro.medium.com/max/448/1*6SYpqNy07GJMg4lmhOlqaw.png)

其中 α 是一个平滑因子，取值介于 0 和 1 之间，包括 1：

![img](https://miro.medium.com/max/220/1*FmycM1QxyWn_ogHyqA6e0Q.png)

当adjust parameters为真时，yt 为：

![img](https://miro.medium.com/max/988/1*Tvsp2kZfWBxGHPqLHsWWDw.png)

当adjust parameters为假时，yt 为：

![img](https://miro.medium.com/max/514/1*peh1QtPGvtMJnd18_raGGw.png)

示例如下：

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.randn(10, 4),
   index = pd.date_range('12/1/2022', periods=10),
   columns = ['A', 'B', 'C', 'D'])
#设置com=0.5，先加权再求均值
print(df.ewm(com=0.5).mean())
```

输出结果：

```
                   A         B         C         D
2022-12-01 -0.263089  1.707143 -0.211812  0.839371
2022-12-02 -0.354277  0.574824  0.398160 -0.400637
2022-12-03  0.544446 -0.110859  0.384631 -0.520709
2022-12-04  0.248907  1.568154 -0.221049 -0.433587
2022-12-05 -1.216711 -0.632707  0.013386 -1.018607
2022-12-06 -0.680604 -0.954285 -0.744970 -0.761510
2022-12-07  0.997426 -0.694479 -0.799153  0.151936
2022-12-08  1.288902 -0.057421  0.297194  0.136069
2022-12-09  0.507135  0.436842  0.020831 -0.917106
2022-12-10 -0.110207  0.253735  0.165642 -0.577304
```

在数据分析的过程中，使用窗口函数能够提升数据的准确性，并且**使数据曲线的变化趋势更加平滑**，从而让数据分析变得更加准确、可靠。