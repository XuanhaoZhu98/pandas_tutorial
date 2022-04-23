# 14 Pandas设置数据显示格式

在用 Pandas 做数据分析的过程中，总需要打印数据分析的结果，如果数据体量较大就会存在输出内容不全（部分内容省略）或者换行错误等问题。Pandas 为了解决上述问题，允许你对数据显示格式进行设置。下面列出了五个用来设置显示格式的函数，分别是：

- get_option()
- set_option()
- reset_option()
- describe_option()
- option_context()


它们的功能介绍如下：

| 函数名称        | 说明                                                     |
| --------------- | -------------------------------------------------------- |
| get_option      | 获取解释器的默认参数值。                                 |
| set_option      | 更改解释器的默认参数值。                                 |
| reset_option    | 解释器的参数重置为默认值。                               |
| describe_option | 输出参数的描述信息。                                     |
| option_context  | 临时设置解释器参数，当退出使用的语句块时，恢复为默认值。 |


下面对上述函数分别进行介绍。

## get_option()

该函数接受单一参数，用来获取显示相关显示选项，示例如下：

#### 1) display.max_rows

获取显示上限的行数，示例如下：

```python
import pandas as pd
print (pd.get_option("display.max_rows"))
```

输出结果：

```
60
```

#### 2) display.max_columns

获取显示上限的列数，示例如下：

```python
import pandas as pd
print (pd.get_option("display.max_columns"))
```

输出结果：

```
20
```

由此可知，默认值显示上限是（60，20）。

## set_option()

该函数用来更改相关显示选项，示例如下：

#### 1) 修改默认行数

```python
import pandas as pd
pd.set_option("display.max_rows",70)
print (pd.get_option("display.max_rows"))
```

输出结果：

```
70
```

#### 2) 修改默认列数

```python
import pandas as pd
pd.set_option("display.max_columns",40)
print (pd.get_option("display.max_columns"))
```

输出结果：

```
40
```

## reset_option()

该方法接受一个参数，并将修改后的相关显示选项设置回默认值。示例如下：

```python
import pandas as pd
pd.reset_option("display.max_rows")
#恢复为默认值
print(pd.get_option("display.max_rows"))
```

输出结果：

```
60
```

## describe_option()

该方法输出参数的描述信息。示例如下：

```python
import pandas as pd
pd.describe_option("display.max_rows")
```

输出结果：

```
display.max_rows : int
    If max_rows is exceeded, switch to truncate view. Depending on
    `large_repr`, objects are either centrally truncated or printed as
    a summary view. 'None' value means unlimited.

    In case python/IPython is running in a terminal and `large_repr`
    equals 'truncate' this can be set to 0 and pandas will auto-detect
    the height of the terminal and print a truncated object which fits
    the screen height. The IPython notebook, IPython qtconsole, or
    IDLE do not run in a terminal and hence it is not possible to do
    correct auto-detection.
    [default: 60] [currently: 60]
```

## option_context()

option_context() 上下文管理器，用于临时设置 with 语句块中的默认显示参数。当您退出 with 语句块时，参数值会自动恢复。示例如下：

```python
import pandas as pd
with pd.option_context("display.max_rows",10):
	print(pd.get_option("display.max_rows"))
print(pd.get_option("display.max_rows"))
```

输出结果：

```
10
60
```


注意：第一个 Print 语句打印 option_context() 设置的临时值。当退出 with 语句块时，第二个 Print 语句打印解释器默认值。

## 常用参数项

最后，对上述函数常用的参数项做以下总结：

| 参数                      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| display.max_rows          | 最大显示行数，超过该值用省略号代替，为None时显示所有行。     |
| display.max_columns       | 最大显示列数，超过该值用省略号代替，为None时显示所有列。     |
| display.expand_frame_repr | 输出数据宽度超过设置宽度时，表示是否对其要折叠，False不折叠，True要折叠。 |
| display.max_colwidth      | 单列数据宽度，以字符个数计算，超过时用省略号表示。           |
| display.precision         | 设置输出数据的小数点位数。                                   |
| display.width             | 数据显示区域的宽度，以总字符数计算。                         |
| display.show_dimensions   | 当数据量大需要以truncate（带引号的省略方式）显示时，该参数表示是否在最后显示数据的维数，默认 True 显示，False 不显示。上述参数项，基本上可以满足我们的日常需求。 |
| display.large_repr        | 当数据维度超过max_rows和max_columns时，设置数据的显示方式，参数值truncate显示带省略号的数据（默认方式）；参数值info显示数据的统计信息，而不直接显示数据（info实际就是df.info()函数） |
| display.max_info_columns  | df.info()函数按列统计每列的非空数据个数，当数据很大时，计算过程非常慢，该参数设置最大列数，当数据表的列数小于该值时，才计算每列的非空值并输出，当超过该值时不进行计算。 |