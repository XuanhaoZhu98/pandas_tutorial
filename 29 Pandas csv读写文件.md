# 28 Pandas csv读写文件

我们知道，文件的读写操作属于计算机的 IO 操作，Pandas IO 操作提供了一些读取器函数，比如 pd.read_csv()、pd.read_json 等，它们都返回一个 Pandas 对象。

在 Pandas 中用于读取文本的函数有两个，分别是： read_csv() 和 read_table() ，它们能够自动地将表格数据转换为 DataFrame 对象。其中 read_csv 的语法格式，如下：

```python
pandas.read_csv(filepath_or_buffer, sep=',', delimiter=None, header='infer',names=None, index_col=None, usecols=None)
```

下面，新建一个 txt 文件，并添加以下数据：

```
Name,Hire Date,Salary,Leaves Remaining 
John Idle,08/15/14,50000.00,10 
Smith Gilliam,04/07/15,65000.00,6 
Parker Chapman,02/21/14,45000.00,7 
Jones Palin,10/14/13,70000.00,3 
Terry Gilliam,07/22/14,48000.00,9 
Michael Palin,06/28/13,66000.00,8 
```

将 txt 文件另存为 employee information.csv 文件格式，直接修改文件扩展名即可。接下来，对此文件进行操作。

## read_csv()

read_csv() 表示从 CSV 文件中读取数据，并创建 DataFrame 对象。

```python
import pandas as pd
#需要注意文件的路径
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv")
print (df)
```

输出结果：

```
   ID            Name           Hire Date   Salary  Leaves Remaining
0   1       John Idle   8/1/2014 00:00:00  50000.0                10
1   2   Smith Gilliam   4/7/2015 00:00:00  65000.0                 6
2   3  Parker Chapman   2/1/2014 00:00:00  45000.0                 7
3   4     Jones Palin  10/1/2013 00:00:00  70000.0                 3
4   5   Terry Gilliam   7/1/2014 00:00:00  48000.0                 9
5   6   Michael Palin   6/1/2013 00:00:00  66000.0                 8
```

#### 1) 自定义索引

在 CSV 文件中指定了一个列，然后使用`index_col`可以实现自定义索引。

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv",index_col=['ID'])
print(df)
```

输出结果：

```
              Name           Hire Date   Salary  Leaves Remaining
ID                                                               
1        John Idle   8/1/2014 00:00:00  50000.0                10
2    Smith Gilliam   4/7/2015 00:00:00  65000.0                 6
3   Parker Chapman   2/1/2014 00:00:00  45000.0                 7
4      Jones Palin  10/1/2013 00:00:00  70000.0                 3
5    Terry Gilliam   7/1/2014 00:00:00  48000.0                 9
6    Michael Palin   6/1/2013 00:00:00  66000.0                 8
```

#### 2) 查看每一列的dtype

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv",dtype={'Salary':np.int64})
print(df.dtypes)
```

输出结果：

```
ID                   int64
Name                object
Hire Date           object
Salary               int64
Leaves Remaining     int64
dtype: object
```

注意：默认情况下，Salary 列的 dtype 是 float 类型，但结果显示其为 int 类型，因为我们已经在上述代码中做了类型转换。

#### 3) 更改文件标头名

使用 names 参数可以指定头文件的名称。

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv",names=['a','b','c','d','e'])
print(df)
```

输出结果：

```
    a               b                   c         d                 e
0  ID            Name           Hire Date    Salary  Leaves Remaining
1   1       John Idle   8/1/2014 00:00:00  50000.00               10 
2   2   Smith Gilliam   4/7/2015 00:00:00  65000.00                6 
3   3  Parker Chapman   2/1/2014 00:00:00  45000.00                7 
4   4     Jones Palin  10/1/2013 00:00:00  70000.00                3 
5   5   Terry Gilliam   7/1/2014 00:00:00  48000.00                9 
6   6   Michael Palin   6/1/2013 00:00:00  66000.00               8  
```

注意：文件标头名是附加的自定义名称，但是您会发现，原来的标头名（列标签名）并没有被删除，此时您可以使用`header`参数来删除它。

通过传递标头所在行号实现删除，如下所示：

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv",names=['a','b','c','d','e'],header=0)
print(df)
```

输出结果：

```
   a               b                   c        d   e
0  1       John Idle   8/1/2014 00:00:00  50000.0  10
1  2   Smith Gilliam   4/7/2015 00:00:00  65000.0   6
2  3  Parker Chapman   2/1/2014 00:00:00  45000.0   7
3  4     Jones Palin  10/1/2013 00:00:00  70000.0   3
4  5   Terry Gilliam   7/1/2014 00:00:00  48000.0   9
5  6   Michael Palin   6/1/2013 00:00:00  66000.0   8
```

假如原标头名并没有定义在第一行，您也可以传递相应的行号来删除它。

#### 4) 跳过文件头部行数

`skiprows`参数表示跳过指定的行数。

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv",skiprows = 2)
print(df)
```

输出结果：

```
   2   Smith Gilliam   4/7/2015 00:00:00  65000.00  6 
0  3  Parker Chapman   2/1/2014 00:00:00   45000.0   7
1  4     Jones Palin  10/1/2013 00:00:00   70000.0   3
2  5   Terry Gilliam   7/1/2014 00:00:00   48000.0   9
3  6   Michael Palin   6/1/2013 00:00:00   66000.0   8
```

注意：包含标头所在行。

#### 5) 跳过文件底部行数

`skipfooter`参数表示跳过指定的行数。

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv",skipfooter = 2)
print(df)
```

输出结果：

```
   ID            Name           Hire Date   Salary  Leaves Remaining
0   1       John Idle   8/1/2014 00:00:00  50000.0                10
1   2   Smith Gilliam   4/7/2015 00:00:00  65000.0                 6
2   3  Parker Chapman   2/1/2014 00:00:00  45000.0                 7
3   4     Jones Palin  10/1/2013 00:00:00  70000.0                 3
```

#### 6) 设置引擎

可以设置要使用的解析器引擎。C 和 pyarrow 引擎更快，而 python 引擎目前功能更完整。目前仅 pyarrow 引擎支持多线程。

```python
import pandas as pd
df=pd.read_csv("C:/Users/xuanhao/Desktop/employee information.csv", engine='python')
```

## to_csv()

Pandas 提供的 to_csv() 函数用于将 DataFrame 转换为 CSV 数据。如果想要把 CSV 数据写入文件，只需向函数传递一个文件对象即可。否则，CSV 数据将以字符串格式返回。

下面看一组简单的示例：

```python
import pandas as pd 
data = {'Name': ['Smith', 'Parker'], 'ID': [101, 102], 'Language': ['Python', 'JavaScript']} 
info = pd.DataFrame(data) 
print('DataFrame Values:\n', info) 
#转换为csv数据
csv_data = info.to_csv() 
print('\nCSV String Values:\n', csv_data)  
```

输出结果：

```
DataFrame Values:
      Name   ID    Language
0   Smith  101      Python
1  Parker  102  JavaScript

CSV String Values:
 ,Name,ID,Language
0,Smith,101,Python
1,Parker,102,JavaScript
```

指定 CSV 文件输出时的分隔符，并将其保存在 pandas.csv 文件中，代码如下：

```python
import pandas as pd
#注意：pd.NaT表示null缺失数据
data = {'Name': ['Smith', 'Parker'], 'ID': [101, pd.NaT], 'Language': ['Python', 'JavaScript']}
info = pd.DataFrame(data)
csv_data = info.to_csv("C:/Users/xuanhao/Desktop/pandas.csv",sep='|')
```