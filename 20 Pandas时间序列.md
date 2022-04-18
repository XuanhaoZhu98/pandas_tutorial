# 20 Pandas时间序列

顾名思义，时间序列（time series），就是由时间构成的序列，它指的是在一定时间内按照时间顺序测量的某个变量的取值序列，比如一天内的温度会随时间而发生变化，或者股票的价格会随着时间不断的波动，这里用到的一系列时间，就可以看做时间序列。时间序列包含三种应用场景，分别是：

- 特定的时刻（timestamp），也就是时间戳；
- 固定的时期（period），比如某年某月，表示一段时间区间；
- 时间间隔（interval），每隔一段时间具有规律性；
  在处理时间序列的过程中，我们一般会遇到两个问题，第一，如何创建时间序列；第二，如何更改已生成时间序列的频率。 Pandas 为解决上述问题提供了一套简单、易用的方法。

下面用 Python 内置的 datetime 模块来获取当前时间，通过该模块提供的`now()`方法即可实现。

```python
from datetime import datetime
#数据类型为datetime
print(datetime.now())
```

输出结果：

```
2022-04-16 19:20:13.461152
```

## 创建时间戳

TimeStamp（时间戳） 是时间序列中的最基本的数据类型，它将数值与时间点完美结合在一起。Pandas 使用下列方法创建时间戳：

```python
import pandas as pd
print (pd.Timestamp('2022-03-01'))
```

输出结果：

```
2022-03-01 00:00:00
```

同样，可以将整型或浮点型表示的时间转换为时间戳。默认的单位是纳秒(时间戳单位)，示例如下：

```python
print(pd.Timestamp(1650764255,unit='s'))
#使用to_datetime有同样效果
print(pd.to_datetime(1650764255, unit='s'))
```

输出结果：

```
2022-04-24 01:37:35
2022-04-24 01:37:35
```

## 创建时间范围

通过 date_range() 方法可以创建某段连续的时间或者固定间隔的时间时间段。该函数的默认频率为 "D"， 也就是“天”。日期序列只包含年、月、日，不包含时、分、秒。

该函数提供了三个参数，分别是：

- start：开始时间
- end：结束时间
- freq：时间频率，默认为 "D"（天），可以是’H’、‘D’、‘M’、‘5H’、‘10D’等
- inclusive：可以选择“both”, “neither”, “left”, “right”，是否将每个边界设置为关闭或打开
- tz：可以设置时区

当我们使用 date_range() 来创建日期范围时，该函数包含结束的日期，用数学术语来说就是区间**左闭右闭**，即包含起始值，也包含结束值。示例如下：

```python
import pandas as pd
#建议使用datetime模块创建时间
start = datetime(2022, 1, 1)
end = datetime(2022, 1, 5)
print(pd.date_range(start,end))
```

输出结果：

```
DatetimeIndex(['2022-01-01', '2022-01-02', '2022-01-03', '2022-01-04',
               '2022-01-05'],
              dtype='datetime64[ns]', freq='D')
```

指定起止时间：

```python
import pandas as pd
#默认频率为Day
print(pd.date_range(start='1/1/2022', end='1/08/2022'))
```

输出结果：

```
DatetimeIndex(['2022-01-01', '2022-01-02', '2022-01-03', '2022-01-04',
               '2022-01-05', '2022-01-06', '2022-01-07', '2022-01-08'],
              dtype='datetime64[ns]', freq='D')
```

指定开始时间和时间序列数量：

```python
import pandas as pd
#默认频率为Day
print(pd.date_range('20220101', periods=10))
```

输出结果：

```
DatetimeIndex(['2022-01-01', '2022-01-02', '2022-01-03', '2022-01-04',
               '2022-01-05', '2022-01-06', '2022-01-07', '2022-01-08',
               '2022-01-09', '2022-01-10'],
              dtype='datetime64[ns]', freq='D')
```

设置时区：

```python
print(pd.date_range(start='1/1/2022', periods=5, tz='Europe/London'))
```

输出结果：

```
DatetimeIndex(['2022-01-01 00:00:00+00:00', '2022-01-02 00:00:00+00:00',
               '2022-01-03 00:00:00+00:00', '2022-01-04 00:00:00+00:00',
               '2022-01-05 00:00:00+00:00'],
              dtype='datetime64[ns, Europe/London]', freq='D')
```

指定结束时间、时间序列数量、频率和包含参数：

```python
import pandas as pd
#默认频率为Day
print(pd.date_range(end='20220110',periods=10,freq='2D',inclusive='both'))
```

输出结果：

```
DatetimeIndex(['2021-12-23', '2021-12-25', '2021-12-27', '2021-12-29',
               '2021-12-31', '2022-01-02', '2022-01-04', '2022-01-06',
               '2022-01-08', '2022-01-10'],
              dtype='datetime64[ns]', freq='2D')
```

使用分钟频率输出：

```python
import pandas as pd
#freq表示时间频率，每30min变化一次
print(pd.date_range("9:00", "18:10", freq="30min").time)
```

输出结果：

```
[datetime.time(9, 0) datetime.time(9, 30) datetime.time(10, 0)
 datetime.time(10, 30) datetime.time(11, 0) datetime.time(11, 30)
 datetime.time(12, 0) datetime.time(12, 30) datetime.time(13, 0)
 datetime.time(13, 30) datetime.time(14, 0) datetime.time(14, 30)
 datetime.time(15, 0) datetime.time(15, 30) datetime.time(16, 0)
 datetime.time(16, 30) datetime.time(17, 0) datetime.time(17, 30)
 datetime.time(18, 0)]
```

bdate_range() 表示创建工作日的日期范围，它与 date_range() 不同，它不包括周六、周日。

```python
import pandas as pd
print(pd.bdate_range('11/25/2022', periods=8))
```

输出结果：

```
DatetimeIndex(['2022-11-25', '2022-11-28', '2022-11-29', '2022-11-30',
               '2022-12-01', '2022-12-02', '2022-12-05', '2022-12-06'],
              dtype='datetime64[ns]', freq='B')
```

上述方法中，date_range() 默认频率是日历日，而 bdate_range() 的默认频率是工作日。

## 时间序列属性

pandas的时间序列DatetimeIndex有以下属性，可以用来进行特定时间的提取

```python
import pandas as pd
#修改为按小时
print(pd.date_range("6:10", "11:45", freq="H").time)
print(pd.date_range('20200416',periods=5, freq="D").day)
print(pd.date_range('20200416',periods=5, freq="M").month)
print(pd.date_range('20200416',periods=5, freq="Y").year)
```

输出结果：

```
[datetime.time(6, 10) datetime.time(7, 10) datetime.time(8, 10)
 datetime.time(9, 10) datetime.time(10, 10) datetime.time(11, 10)]
 
Int64Index([16, 17, 18, 19, 20], dtype='int64')

Int64Index([4, 5, 6, 7, 8], dtype='int64')

Int64Index([2020, 2021, 2022, 2023, 2024], dtype='int64')
```

| 属性        | 说明                                              |
| ----------- | ------------------------------------------------- |
| year        | 日期时间的年份。                                  |
| month       | 日期时间的月份，一月=1，十二月=12。               |
| day         | 日期时间的日期。                                  |
| hour        | 日期时间的小时数。                                |
| minute      | 日期时间的分钟数。                                |
| second      | 日期时间的秒数。                                  |
| microsecond | 日期时间的微秒。                                  |
| nanosecond  | 日期时间的纳秒。                                  |
| date        | datetime.date返回 python对象的 numpy 数组。       |
| time        | 返回 numpy datetime.time 对象数组。               |
| timetz      | datetime.time 返回具有时区信息的 numpy 对象数组。 |

## 转化为日期对象

您可以使用 to_datetime() 函数将 series 或 list 转换为日期对象，其中 list 会转换为`DatetimeIndex`。示例如下：

```python
import pandas as pd
timeseries = pd.Series(['Jun 3, 2022','2022-12-10', None])
print(pd.to_datetime(timeseries))
```

输出结果：

```
0   2022-06-03
1   2022-12-10
2          NaT
dtype: datetime64[ns]
```

注意：NaT 表示的不是时间 ，它等效于 NaN。

最后再来看一个示例：

```python
import pandas as pd
#传入list，生成Datetimeindex
timelist = ['Jun 01, 2020','2020-12-10', None]
print(pd.to_datetime(timelist))
```

输出结果：

```
DatetimeIndex(['2020-06-01', '2020-12-10', 'NaT'], dtype='datetime64[ns]', freq=None)
```

## 创建时间周期

Time Periods 表示时间跨度，一段时间周期，它被定义在 Pandas Periods 类中，时期表示的是时间区间，比如数日、数月、数季、数年等。我们可以使用 period_range() 方法来创建时间周期范围。示例如下：

```python
import pandas as pd
#Y表示年
p = pd.period_range('2020','2022', freq='Y')
p
```

输出结果：

```
PeriodIndex(['2020', '2021', '2022'], dtype='period[A-DEC]')
```

A-DEC中，A表示年终频率，DEC表示以12月作为结束的一整年。

PeriodIndex类的构造函数允许直接使用一组字符串表示一段时期:

```python
import pandas as pd
values = ['2022Q3','2022Q2','2022Q1']
index = pd.PeriodIndex(values,freq='Q-DEC')
index
```

输出结果：

```
PeriodIndex(['2022Q3', '2022Q2', '2022Q1'], dtype='period[Q-DEC]')
```

period_range函数可用于创建规则的时期范围：

```python
import pandas as pd
#创建从2022-01-01到2022-06-30所有月份的Period
rng = pd.period_range('1/1/2022','6/30/2022',freq='M') 
pd.Series(np.random.randn(6),index=rng)
```

输出结果：

```
2022-01    0.629688
2022-02    1.401240
2022-03   -0.663876
2022-04   -1.144239
2022-05    1.132210
2022-06   -2.074965
Freq: M, dtype: float64
```

## 频率和周期转换

通过该类提供的方法可以实现将频率转换为周期。比如 Periods() 方法，可以将频率 "M"（月）转换为 Period（时间段）。

下面示例，使用 asfreq() 和 start 参数，打印 "01" ，若使用 end 参数，则打印 "31"。示例如下：

```python
import pandas as pd 
x = pd.Period('2022', freq='M')
#将频率月转换为日（第一天）
print(x.asfreq('D', how='start'))
#将频率月转换为日（最后一天）
print(x.asfreq('D', how='end'))
```

输出结果：

```
2022-01
2022-01-01
2022-01-31
```

季度周期频度：

```python
import pandas as pd 
# Q-JAN中，Q表示季度频率，JAN表示以1月作为结束的一个季度
p = pd.Period('2022Q4', freq='Q-JAN')
p
p.asfreq('D', 'start')
p.asfreq('D', 'end')
```

输出结果：

```
Period('2022Q4', 'Q-JAN')
Period('2021-11-01', 'D')
Period('2022-01-31', 'D')
```

不同频率经过asfreq()转换后的结果不同：

```python
import pandas as pd 
#2021年7月1日到2022年6月30日
p = pd.Period('2022',freq='A-JUN') 
print(p)
print(p.asfreq('D','start'))
print(p.asfreq('D','end'))
```

输出结果：

```
2022
2021-07-01
2022-06-30
```

从高频率转换为低频率时，超时期(较大的时期)是由子时期(较小的时期)的位置决定的：

```python
import pandas as pd 
#2021年7月1日到2022年6月30日
p = pd.Period('2021-08','M')
print(p)
# 202108对于频率A-JUN是属于2022年度的
print(p.asfreq('A-JUN'))
```

输出结果：

```
2021-08
2022
```

对于PeriodIndex或TimeSeries的频率转换方式相同：

```python
import pandas as pd 
rng = pd.period_range('2019','2022',freq='A-DEC')
ts = pd.Series(np.random.randn(len(rng)),index=rng)
print(ts)
print(ts.asfreq('M',how='start'))
print(ts.asfreq('B',how='end'))
```

输出结果：

```
2019    1.947783
2020   -0.550404
2021    0.124142
2022    1.169237
Freq: A-DEC, dtype: float64

2019-01    1.947783
2020-01   -0.550404
2021-01    0.124142
2022-01    1.169237
Freq: M, dtype: float64

2019-12-31    1.947783
2020-12-31   -0.550404
2021-12-31    0.124142
2022-12-30    1.169237
Freq: B, dtype: float64
```

对于常用的时间序列频率，Pandas 为其规定了一些字符串别名，我们将这些别名称为“offset（偏移量）”。如下表所示：

| 别名 | 描述             | 别名  | 描述             |
| ---- | ---------------- | ----- | ---------------- |
| B    | 工作日频率       | BQS   | 工作季度开始频率 |
| D    | 日历日频率       | A     | 年终频率         |
| W    | 每周频率         | BA    | 工作年度结束频率 |
| M    | 月末频率         | BAS   | 工作年度开始频率 |
| SM   | 半月结束频率     | BH    | 营业时间频率     |
| BM   | 工作月结束频率   | H     | 小时频率         |
| MS   | 月开始频率       | T,min | 每分钟频率       |
| SMS  | 半月开始频率     | S     | 每秒钟频率       |
| BMS  | 工作月开始频率   | L,ms  | 毫秒             |
| Q    | 季末频率         | U,us  | 微妙             |
| BQ   | 工作季度结束频率 | N     | 纳秒             |
| QS   | 季度开始频率     |       |                  |

## 时间周期计算

周期计算，指的是对时间周期进行算术运算，所有的操作将在“频率”的基础上执行。

```python
import pandas as pd 
#S表示秒
x = pd.Period('2022', freq='S')  
x 
```

输出结果：

```
Period('2022-01-01 00:00:00', 'S')
```

通过加减整数可以实现对Period的移动，执行计算示例：

```python
#加1s的时间
x+1
```

输出结果：

```
Period('2014-01-01 00:00:01', 'S')
```

再看一组完整的示例：

```python
#定义时期period，默认freq="Y"年份
p1=pd.Period('2022')
p2=pd.Period('2021')
#使用f''格式化输出
print(f'p1={p1}')
print(f'p2={p2}')
print(f'p1 and p2 are separated by {p1-p2} years')
#f''表示字符串格式化输出
print(f'Five years ago was {p1-5} years')
```

输出结果：

```
p1=2022
p2=2021
p1 and p2 are separated by <YearEnd: month=12> years
Five years ago was 2017 years
```

## 时间序列转换

单个时间戳和时间序列间转换：

```python
print(pd.Period('2022-04-1 08:02:35',freq='S').to_timestamp())
type(pd.Period('2022-04-1 08:02:35',freq='S').to_timestamp())
print(pd.Timestamp('2022-04-01 08:02:35').to_period('S'))
type(pd.Timestamp('2022-04-01 08:02:35').to_period('S'))
```

输出结果：

```
2022-04-01 08:02:35
pandas._libs.tslibs.timestamps.Timestamp
2022-04-01 08:02:35
pandas._libs.tslibs.period.Period
```

时间戳转化为时间周期：

```python
ts_index=pd.date_range("2022-03-17","2022-03-18",freq="2H")
print(ts_index)
pd_index=ts_index.to_period('H')
print(pd_index)
```

输出结果：

```
DatetimeIndex(['2022-03-17 00:00:00', '2022-03-17 04:00:00',
               '2022-03-17 08:00:00', '2022-03-17 12:00:00',
               '2022-03-17 16:00:00', '2022-03-17 20:00:00',
               '2022-03-18 00:00:00'],
              dtype='datetime64[ns]', freq='4H')
PeriodIndex(['2022-03-17 00:00', '2022-03-17 04:00', '2022-03-17 08:00',
             '2022-03-17 12:00', '2022-03-17 16:00', '2022-03-17 20:00',
             '2022-03-18 00:00'],
            dtype='period[H]')
```

时间戳转化为时间周期：

```python
pd_index=pd.period_range("2022-03-17","2022-03-18",freq="4H")
print(pd_index)
ts_index=pd_index.to_timestamp('H')
print(ts_index)
```

输出结果：

```
PeriodIndex(['2022-03-17 00:00', '2022-03-17 04:00', '2022-03-17 08:00',
             '2022-03-17 12:00', '2022-03-17 16:00', '2022-03-17 20:00',
             '2022-03-18 00:00'],
            dtype='period[4H]')
DatetimeIndex(['2022-03-17 00:00:00', '2022-03-17 04:00:00',
               '2022-03-17 08:00:00', '2022-03-17 12:00:00',
               '2022-03-17 16:00:00', '2022-03-17 20:00:00',
               '2022-03-18 00:00:00'],
              dtype='datetime64[ns]', freq='4H')
```

相互转换示例如下：

```python
#时间戳与时期之间的转换：pd.to_period()、pd.to_timestamp()
rng = pd.date_range('2022/1/1', periods = 10, freq = 'M')
prng = pd.period_range('2021','2022', freq = 'M')

#每月最后一日，转化为每月
ts1 = pd.Series(np.random.rand(len(rng)), index = rng)
print(ts1.head())
print(ts1.to_period().head())

#每月，转化为每月第一天
ts2 = pd.Series(np.random.rand(len(prng)), index = prng)
print(ts2.head())
print(ts2.to_timestamp().head())
```

输出结果：

```
2022-01-31    0.358962
2022-02-28    0.459514
2022-03-31    0.777623
2022-04-30    0.927882
2022-05-31    0.701045
Freq: M, dtype: float64
2022-01    0.358962
2022-02    0.459514
2022-03    0.777623
2022-04    0.927882
2022-05    0.701045
Freq: M, dtype: float64

2021-01    0.056592
2021-02    0.894992
2021-03    0.434976
2021-04    0.254169
2021-05    0.622583
Freq: M, dtype: float64
2021-01-01    0.056592
2021-02-01    0.894992
2021-03-01    0.434976
2021-04-01    0.254169
2021-05-01    0.622583
Freq: MS, dtype: float64
```

## 时间序列作为索引

时间序列做为索引，生成Series一维数组：

```python
import pandas as pd
dates = pd.date_range(start='20220101',periods=5,freq='2D')
print(pd.Series(range(10,20,2),index=dates))
```

输出结果：

```
2022-01-01    10
2022-01-03    12
2022-01-05    14
2022-01-07    16
2022-01-09    18
Freq: 2D, dtype: int64
```

时间序列做行索引，生成DateFrame二维数组：

```python
import pandas as pd
dates = pd.date_range(start='20220101',periods=5,freq='2D')
print(pd.DataFrame(np.random.randn(5,5), index=dates, columns=list('ABCDE')))
```

输出结果：

```
                   A         B         C         D         E
2022-01-01  0.315836 -2.525057  0.644021 -0.787891  0.674636
2022-01-03 -0.113040 -0.746247  1.255475  0.477652 -0.530692
2022-01-05 -0.586724  1.790907 -0.577267 -0.804777 -1.323708
2022-01-07  1.287829  1.503082 -0.232731 -0.066120 -1.355111
2022-01-09  0.960016  0.703791 -1.087694 -0.429778  0.080602
```

