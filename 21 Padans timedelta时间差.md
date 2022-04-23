# 21 Padans Timedelta时间差

Timedelta 表示时间差（或者时间增量），我们可以使用不同的时间单位来表示它，比如，天、小时、分、秒。时间差的最终的结果可以是正时间差，也可以是负时间差。

本节主要介绍创建 Timedelta （时间差）的方法以及与时间差相关的运算法则。

## 字符串

通过传递字符串可以创建 Timedelta 对象，示例如下：

```python
import pandas as pd
print(pd.Timedelta('5 days 8 hours 6 minutes 59 seconds'))
#U微妙、N纳秒、L毫秒、S秒、T分钟、D天、W周
print(pd.Timedelta('1W 1D 1T  1S 1L 1U 1N'))
```

输出结果：

```
5 days 08:06:59
8 days 00:01:01.001001001
```

## 整数

通过传递整数值和`unit`参数也可以创建一个 Timedelta 对象。

```python
import pandas as pd
print(pd.Timedelta(19,unit='h'))
print(pd.Timedelta(1, "d"))
```

输出结果：

```
0 days 19:00:00
1 days 00:00:00
```

## 数据偏移量

数据偏移量， 比如，周(weeks)、天(days)、小时(hours)、分钟(minutes)、秒(milliseconds)、毫秒(milliseconds)、微秒(microseconds)、纳秒(nanoseconds)都可以使用。

```python
import pandas as pd
print (pd.Timedelta(days=2,hours=6))
```

输出结果：

```
2 days 06:00:00
```

## to_timedelta()

您可以使用`pd.to_timedelta()`方法，将具有 timedelta 格式的值 (标量、数组、列表或 Series）转换为 Timedelta 类型。如果输入是 Series，则返回 Series；如果输入是标量，则返回值也为标量，其他情况输出 TimedeltaIndex。示例如下：

```python
import pandas as pd
#解析字符串列表或数组
timedeltalist=['1 days 06:05:01.00003', '15.5us', 'nan']
print(pd.to_timedelta(timedeltalist))
#通过指定unit关键字参数来转换数字
print(pd.to_timedelta(np.arange(5), unit='s'))
```

输出结果：

```
TimedeltaIndex(['1 days 06:05:01.000030', '0 days 00:00:00.000015500', NaT], dtype='timedelta64[ns]', freq=None)

TimedeltaIndex(['0 days 00:00:00', '0 days 00:00:01', '0 days 00:00:02',
                '0 days 00:00:03', '0 days 00:00:04'],
               dtype='timedelta64[ns]', freq=None)
```

## 算术操作

通过对`datetime64[ns]`类型的时间序列或时间戳做算术运算，其运算结果依然是`datetime64[ns]`数据类型。接下来，我们创建一个带有 Timedelta 与 datetime 的 DataFrame 对象，并对其做一些算术运算。

```python
import pandas as pd
s = pd.Series(pd.date_range('2022-1-1', periods=5, freq='D'))
#推导式用法
td = pd.Series([ pd.Timedelta(days=i) for i in range(5)])
df = pd.DataFrame(dict(A = s, B = td))
print(df)
```

输出结果：

```
           A      B
0 2022-01-01 0 days
1 2022-01-02 1 days
2 2022-01-03 2 days
3 2022-01-04 3 days
4 2022-01-05 4 days
```

### 加法运算

```python
#加法运算
df['C']=df['A']+df['B']
print(df)
```

输出结果：

```
           A      B          C
0 2022-01-01 0 days 2022-01-01
1 2022-01-02 1 days 2022-01-03
2 2022-01-03 2 days 2022-01-05
3 2022-01-04 3 days 2022-01-07
4 2022-01-05 4 days 2022-01-09
```

### 减法运算

```python
df['D']=df['C']-df['B']
print(df)
```

输出结果：

```
           A      B          C          D
0 2022-01-01 0 days 2022-01-01 2022-01-01
1 2022-01-02 1 days 2022-01-03 2022-01-02
2 2022-01-03 2 days 2022-01-05 2022-01-03
3 2022-01-04 3 days 2022-01-07 2022-01-04
4 2022-01-05 4 days 2022-01-09 2022-01-05
```