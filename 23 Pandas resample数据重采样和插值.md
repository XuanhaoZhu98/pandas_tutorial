# 23 Pandas resample数据重采样和插值

数据重采样是将时间序列从一个频率转换至另一个频率的过程，它主要有两种实现方式，分别是降采样和升采样，降采样指将高频率的数据转换为低频率，升采样则与其恰好相反，说明如下：

| 方法   | 说明                                       |
| ------ | ------------------------------------------ |
| 降采样 | 将高频率(间隔短)数据转换为低频率(间隔长)。 |
| 升采样 | 将低频率数据转换为高频率。                 |

Pandas 提供了 resample() 函数来实现数据的重采样。

## 降采样

通过 resample() 函数完成数据的降采样，比如按天计数的频率转换为按月计数。

```python
import pandas as pd
import numpy as np
rng = pd.date_range('1/1/2022',periods=100,freq='D')
ts = pd.Series(np.random.randn(len(rng)),index=rng)
#降采样后并聚合
ts.resample('M').mean() 
```

输出结果：

```
2022-01-31   -0.096294
2022-02-28   -0.109322
2022-03-31    0.008780
2022-04-30   -0.131808
Freq: M, dtype: float64
```

如果您只想看到月份，那么您可以设置`kind=period`如下所示：

```python
ts.resample('M',kind='period').mean()
```

输出结果：

```
2022-01   -0.096294
2022-02   -0.109322
2022-03    0.008780
2022-04   -0.131808
Freq: M, dtype: float64
```

## 升采样

升采样是将低频率（时间间隔）转换为高频率，示例如下：

```python
import pandas as pd
import numpy as np
#生成一份时间序列数据
rng = pd.date_range('1/1/2022', periods=20, freq='3D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
print(ts.head())
#使用asfreq()在原数据基础上实现频率转换
ts.resample('D').asfreq().head()
```

输出结果：

```
升采样前：
2022-01-01    1.048531
2022-01-04    0.768341
2022-01-07   -0.044255
2022-01-10    0.245312
2022-01-13    0.018546
Freq: 3D, dtype: float64

升采样后：
2022-01-01    1.048531
2022-01-02         NaN
2022-01-03         NaN
2022-01-04    0.768341
2022-01-05         NaN
Freq: D, dtype: float64
```

## 频率转换

asfreq() 方法不仅能够实现频率转换，还可以保留原频率对应的数值，同时它也可以单独使用，示例如下：

```python
index = pd.date_range('1/1/2022', periods=6, freq='T')
series = pd.Series([0.0, None, 2.0, 3.0,4.0,5.0], index=index)
df = pd.DataFrame({'s':series})
print(df)
print(df.asfreq("45s"))
```

输出结果：

```
                       s
2022-01-01 00:00:00  0.0
2022-01-01 00:01:00  NaN
2022-01-01 00:02:00  2.0
2022-01-01 00:03:00  3.0
2022-01-01 00:04:00  4.0
2022-01-01 00:05:00  5.0

                       s
2022-01-01 00:00:00  0.0
2022-01-01 00:00:45  NaN
2022-01-01 00:01:30  NaN
2022-01-01 00:02:15  NaN
2022-01-01 00:03:00  3.0
2022-01-01 00:03:45  NaN
2022-01-01 00:04:30  NaN
```

## 插值处理

从上述示例不难看出，升采样的结果会产生缺失值，那么就需要对缺失值进行处理，一般有以下几种处理方式：

| 方法                  | 说明                           |
| --------------------- | ------------------------------ |
| pad/ffill             | 用前一个非缺失值去填充缺失值。 |
| backfill/bfill        | 用后一个非缺失值去填充缺失值。 |
| interpolate('linear') | 线性插值方法。                 |
| fillna(value)         | 指定一个值去替换缺失值。       |


下面使用插值方法处理 NaN 值，示例如下：

```python
import pandas as pd
import numpy as np
#创建时间序列数据
rng = pd.date_range('1/1/2022', periods=20, freq='3D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
print(ts.resample('D').asfreq().head())
#使用ffill处理缺失值
ts.resample('D').asfreq().ffill().head()
#使用bfill处理缺失值
ts.resample('D').asfreq().bfill().head()
#使用interpolate处理缺失值
ts.resample('D').asfreq().interpolate('linear').head()
```

输出结果：

```
2022-01-01   -0.640992
2022-01-02         NaN
2022-01-03         NaN
2022-01-04    1.443954
2022-01-05         NaN
Freq: D, dtype: float64


2022-01-01   -0.640992
2022-01-02   -0.640992
2022-01-03   -0.640992
2022-01-04    1.443954
2022-01-05    1.443954
Freq: D, dtype: float64

2022-01-01   -0.640992
2022-01-02    1.443954
2022-01-03    1.443954
2022-01-04    1.443954
2022-01-05    1.787025
Freq: D, dtype: float64

2022-01-01   -0.640992
2022-01-02    0.053990
2022-01-03    0.748972
2022-01-04    1.443954
2022-01-05    1.558311
Freq: D, dtype: float64
```