# 26 Pandas读写文件

当使用 Pandas 做数据分析的时，需要读取事先准备好的数据集，这是做数据分析的第一步。我们知道，文件的读写操作属于计算机的 IO 操作，Pandas IO 操作提供了一些读取器函数，比如 pd.read_csv()、pd.read_json 等，它们都返回一个 Pandas 对象：

- read_csv() 用于读取逗号分隔值 (csv) 文件
- read_table()用于读取通用分隔文件
- read_fwf()用于读取固定宽度格式行的表格
- read_json() 用于读取 json 文件
- read_sql_query() 读取 sql 语句的，


本节将对上述方法做详细介绍。

## CSV文件读写

CSV 又称逗号分隔值文件，是一种简单的文件格式，以特定的结构来排列表格数据。 CSV 文件能够以纯文本形式存储表格数据，比如电子表格、数据库文件，并具有数据交换的通用格式。CSV 文件会在 Excel 文件中被打开，其行和列都定义了标准的数据格式。

将 CSV 中的数据转换为 DataFrame 对象是非常便捷的。和一般文件读写不一样，它不需要你做打开文件、读取文件、关闭文件等操作。相反，您只需要一行代码就可以完成上述所有步骤，并将数据存储在 DataFrame 中。

其中 read_csv 的语法格式，如下：

```python
pandas.read_csv(filepath_or_buffer, sep=',', delimiter=None, header='infer',names=None, index_col=None, usecols=None)
```

下面进行实例演示，首先您需要创建一组数据，并将其保存为 CSV 格式，数据如下：

```
Name,Hire Date,Salary,Leaves Remaining 
John Idle,08/15/14,50000.00,10 
Smith Gilliam,04/07/15,65000.00,6 
Parker Chapman,02/21/14,45000.00,7 
Jones Palin,10/14/13,70000.00,3 
Terry Gilliam,07/22/14,48000.00,9 
Michael Palin,06/28/13,66000.00,8  
```

注意：将上述数据保存到`.txt`的文本文件中，然后将文件的扩展名后缀修改为 csv，即可完成 csv 文件的创建。

### read_csv()

接下来，我们使用下列代码读写数据：

```python
import pandas as pd 
#仅仅一行代码就完成了数据读取，但是注意文件路径不要写错
df = pd.read_csv('C:/Users/xuanhao/Desktop/employee information.csv') 
print(df)  
```

输出结果：

```
             Name Hire Date   Salary  Leaves Remaining 
0       John Idle  08/15/14  50000.0                  10
1   Smith Gilliam  04/07/15  65000.0                   6
2  Parker Chapman  02/21/14  45000.0                   7
3     Jones Palin  10/14/13  70000.0                   3
4   Terry Gilliam  07/22/14  48000.0                   9
5   Michael Palin  06/28/13  66000.0                   8
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

### to_csv()

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

## txt文件读写

### read_table()

read_table()用于读取通用分隔文件，包括csv文件和txt文件，如下所示：

```python
import pandas as pd 
data = pd.read_table('C:/Users/xuanhao/Desktop/employee information.txt', delim_whitespace=True, skiprows=1, skipfooter=1, index_col=0, engine='python')
print(data)
data = pd.read_table('C:/Users/xuanhao/Desktop/employee information.csv',delimiter=',',index_col=0)
print(data)  
```

输出结果：

```
       Name     Hire       Date    Salary   Leaves  Remaining
ID                                                           
1      John     Idle   8/1/2014  00:00:00  50000.0         10
2     Smith  Gilliam   4/7/2015  00:00:00  65000.0          6
3    Parker  Chapman   2/1/2014  00:00:00  45000.0          7
4     Jones    Palin  10/1/2013  00:00:00  70000.0          3
5     Terry  Gilliam   7/1/2014  00:00:00  48000.0          9
6   Michael    Palin   6/1/2013  00:00:00  66000.0          8

              Name           Hire Date   Salary  Leaves Remaining
ID                                                               
1        John Idle   8/1/2014 00:00:00  50000.0                10
2    Smith Gilliam   4/7/2015 00:00:00  65000.0                 6
3   Parker Chapman   2/1/2014 00:00:00  45000.0                 7
4      Jones Palin  10/1/2013 00:00:00  70000.0                 3
5    Terry Gilliam   7/1/2014 00:00:00  48000.0                 9
6    Michael Palin   6/1/2013 00:00:00  66000.0                 8
```

可以看到txt文件中按照空格为分隔符读取了数据，使用read_fwf()来读取是更好的选择。读取csv文件的结果和read_csv类似，但是需要指明分隔符。

### read_fwf()

这个方法可以将固定宽度格式行的表格读入 DataFrame。

```python
import pandas as pd 
data = pd.read_fwf('C:/Users/xuanhao/Desktop/employee information.txt', delim_whitespace=True, skiprows=1, skipfooter=1, index_col=0, engine='python')
print(data)
```

输出结果：

```
                                          Salary  Leaves Remaining
              Name           Hire Date                            
ID                                                                
1        John Idle   8/1/2014 00:00:00   50000.0                10
2    Smith Gilliam   4/7/2015 00:00:00   65000.0                 6
3   Parker Chapman   2/1/2014 00:00:00   45000.0                 7
4      Jones Palin  10/1/2013 00:00:00   70000.0                 3
5    Terry Gilliam   7/1/2014 00:00:00   48000.0                 9
6    Michael Palin   6/1/2013 00:00:00   66000.0                 8
```

这个结果是令人满意的。

## json文件读写

### read_json()

您可以通过下列方法来读取一个 json 文件，如下所示：

```python
import pandas as pd 
data = pd.read_json('C:/Users/xuanhao/Desktop/employee information.json')  
print(data)  
```

输出结果：

```
             Name Hire Date   Salary  Leaves Remaining 
0       John Idle  08/15/14  50000.0                  10
1   Smith Gilliam  04/07/15  65000.0                   6
2  Parker Chapman  02/21/14  45000.0                   7
3     Jones Palin  10/14/13  70000.0                   3
4   Terry Gilliam  07/22/14  48000.0                   9
5   Michael Palin  06/28/13  66000.0                   8
```

### to_json()

Pandas 提供的 to_json() 函数用于将 DataFrame 转换为 CSV 数据。同样的，如果想要把json数据写入文件，只需向函数传递一个文件对象即可。否则，json数据将以字符串格式返回。

下面看一组简单的示例：

```python
#索引和数据分开保存
json_data = info.to_json(orient="split")
print('\nJson String Values:\n', json_data) 
#records编码不会保留index标签。
json_data = info.to_json(orient="records")
print('\nJson String Values:\n', json_data) 
#index编码行标签在外层
json_data = info.to_json(orient="index")
print('\nJson String Values:\n', json_data) 
#columns编码列标签在外层
json_data = info.to_json(orient="columns")
print('\nJson String Values:\n', json_data) 
#values编码只保留数值
json_data = info.to_json(orient="values")
print('\nJson String Values:\n', json_data) 
#table编码保留所有数据
json_data = info.to_json(orient="table")
print('\nJson String Values:\n', json_data) 
#写入文件
json_data = info.to_json("C:/Users/xuanhao/Desktop/pandas.json")
```

输出结果：

```
Json String Values:
 {"columns":["Name","ID","Language"],"index":[0,1],"data":[["Smith",101,"Python"],["Parker",102,"JavaScript"]]}

Json String Values:
 [{"Name":"Smith","ID":101,"Language":"Python"},{"Name":"Parker","ID":102,"Language":"JavaScript"}]

Json String Values:
 {"0":{"Name":"Smith","ID":101,"Language":"Python"},"1":{"Name":"Parker","ID":102,"Language":"JavaScript"}}

Json String Values:
 {"Name":{"0":"Smith","1":"Parker"},"ID":{"0":101,"1":102},"Language":{"0":"Python","1":"JavaScript"}}

Json String Values:
 [["Smith",101,"Python"],["Parker",102,"JavaScript"]]

Json String Values:
 {"schema":{"fields":[{"name":"index","type":"integer"},{"name":"Name","type":"string"},{"name":"ID","type":"integer"},{"name":"Language","type":"string"}],"primaryKey":["index"],"pandas_version":"1.4.0"},"data":[{"index":0,"Name":"Smith","ID":101,"Language":"Python"},{"index":1,"Name":"Parker","ID":102,"Language":"JavaScript"}]}
```



### json输出格式化

```python
import json
json_data = info.to_json(orient="split")
parsed = json.loads(json_data)
print(json.dumps(parsed, indent=4))
```

输出结果：

```
{
    "columns": [
        "Name",
        "ID",
        "Language"
    ],
    "index": [
        0,
        1
    ],
    "data": [
        [
            "Smith",
            101,
            "Python"
        ],
        [
            "Parker",
            102,
            "JavaScript"
        ]
    ]
}
```

## Excel文件读写

Excel 是由微软公司开发的办公软件之一，它在日常工作中得到了广泛的应用。在数据量较少的情况下，Excel 对于数据的处理、分析、可视化有其独特的优势，因此可以显著提升您的工作效率。但是，当数据量非常大时，Excel 的劣势就暴露出来了，比如，操作重复、数据分析难等问题。Pandas 提供了操作 Excel 文件的函数，可以很方便地处理 Excel 表格。

### to_excel()

通过 to_excel() 函数可以将 Dataframe 中的数据写入到 Excel 文件。

如果想要把单个对象写入 Excel 文件，那么必须指定目标文件名；如果想要写入到多张工作表中，则需要创建一个带有目标文件名的`ExcelWriter`对象，并通过`sheet_name`参数依次指定工作表的名称。

to_ecxel() 语法格式如下：

```python
DataFrame.to_excel(excel_writer, sheet_name='Sheet1', na_rep='', float_format=None, columns=None, header=True, index=True, index_label=None, startrow=0, startcol=0, engine=None, merge_cells=True, encoding=None, inf_rep='inf', verbose=True, freeze_panes=None)  
```

下表列出函数的常用参数项，如下表所示：

| 参数名称     | 描述说明                                                     |
| ------------ | ------------------------------------------------------------ |
| excel_wirter | 文件路径或者 ExcelWrite 对象。                               |
| sheet_name   | 指定要写入数据的工作表名称。                                 |
| na_rep       | 缺失值的表示形式。                                           |
| float_format | 它是一个可选参数，用于格式化浮点数字符串。                   |
| columns      | 指要写入的列。                                               |
| header       | 写出每一列的名称，如果给出的是字符串列表，则表示列的别名。   |
| index        | 表示要写入的索引。                                           |
| index_label  | 引用索引列的列标签。如果未指定，并且 hearder 和 index 均为为 True，则使用索引名称。如果 DataFrame 使用 MultiIndex，则需要给出一个序列。 |
| startrow     | 初始写入的行位置，默认值0。表示引用左上角的行单元格来储存 DataFrame。 |
| startcol     | 初始写入的列位置，默认值0。表示引用左上角的列单元格来储存 DataFrame。 |
| engine       | 它是一个可选参数，用于指定要使用的引擎，可以是 openpyxl 或 xlsxwriter。 |


下面看一组简单的示例：

```python
import pandas as pd
#创建DataFrame数据
info_website = pd.DataFrame({'language': ['JAVA', 'JavaScript', 'C++','Python' ],
     'rank': [2, 3, 4, 1],
     'number of jobs': ['29,000', '24,000', '9,000','19,000' ],
     'url': ['www.java.com', 'www.javascript.com', 'www.cplusplus.com','www.python.org' ]})
#创建ExcelWrite对象
writer = pd.ExcelWriter('language.xlsx')
info_website.to_excel(writer)
writer.save()
print('Successfully saved')
```

上述代码执行后会自动生成 language.xlsx 文件，文件内容如下：

![](https://raw.githubusercontent.com/XuanhaoZhu98/image_hosting/main/img/202204181249444.jpg)

图1：DataFrame转为Excel

### read_excel()

如果您想读取 Excel 表格中的数据，可以使用 read_excel() 方法，其语法格式如下：

```python
pd.read_excel(io, sheet_name=0, header=0, names=None, index_col=None,
              usecols=None, squeeze=False,dtype=None, engine=None,
              converters=None, true_values=None, false_values=None,
              skiprows=None, nrows=None, na_values=None, parse_dates=False,
              date_parser=None, thousands=None, comment=None, skipfooter=0,
              convert_float=True, **kwds)
```

下表对常用参数做了说明：

| 参数名称   | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| io         | 表示 Excel 文件的存储路径。                                  |
| sheet_name | 要读取的工作表名称。                                         |
| header     | 指定作为列名的行，默认0，即取第一行的值为列名；若数据不包含列名，则设定 header = None。若将其设置 为 header=2，则表示将前两行作为多重索引。 |
| names      | 一般适用于Excel缺少列名，或者需要重新定义列名的情况；names的长度必须等于Excel表格列的长度，否则会报错。 |
| index_col  | 用做行索引的列，可以是工作表的列名称，如 index_col = '列名'，也可以是整数或者列表。 |
| usecols    | int或list类型，默认为None，表示需要读取所有列。              |
| squeeze    | boolean，默认为False，如果解析的数据只包含一列，则返回一个Series。 |
| converters | 规定每一列的数据类型。                                       |
| skiprows   | 接受一个列表，表示跳过指定行数的数据，从头部第一行开始。     |
| nrows      | 需要读取的行数。                                             |
| skipfooter | 接受一个列表，省略指定行数的数据，从尾部最后一行开始。       |

示例如下所示：

```python
import pandas as pd
#读取excel数据
df = pd.read_excel('language.xlsx',index_col='language',skiprows=[2])
#处理未命名列
df.columns = df.columns.str.replace('Unnamed.*', 'col_label')
print(df)
```

输出结果：

```
          col_label  rank number of jobs                url
language                                                   
JAVA              0     2         29,000       www.java.com
C++               2     4          9,000  www.cplusplus.com
Python            3     1         19,000     www.python.org
```

再看一组示例：

```python
import pandas as pd
#读取excel数据
#index_col选择前两列作为索引列
#选择前三列数据，name列作为行索引
df = pd.read_excel('language.xlsx',index_col=[0,1],usecols=[1,2,3])
#处理未命名列，固定用法
df.columns = df.columns.str.replace('Unnamed.*', 'col_label')
print(df)
```

输出结果：

```
import pandas as pd

#读取excel数据

#index_col选择前两列作为索引列

#选择前三列数据，name列作为行索引

df = pd.read_excel('language.xlsx',index_col=[0,1],usecols=[1,2,3])

#处理未命名列，固定用法

df.columns = df.columns.str.replace('Unnamed.*', 'col_label')

print(df)

                number of jobs
language   rank               
JAVA       2            29,000
JavaScript 3            24,000
C++        4             9,000
Python     1            19,000
```

## SQL数据库读取

如果想要从 SQL 数据库读取数据，首先您应该使用 Python 和数据库建立连接，然后将查询语句传递给 read_sql_query() 方法，下面做简单地演示：

#### 1) 安装pysqlite3模块

```bash
pip install pysqlite3
```

#### 2) 建立数据连接

```python
import sqlite3
con = sqlite3.connect("database.db")  
```

#### 3) 数据库读取数据

在 SQLite 数据库中创建一张信息表，您可以随意添加一些信息，最后使用下列方法读取数据即可：

```python
#con参数指定操作数据库的引擎，可以指定，也可默认
df = pd.read_sql_query("SELECT * FROM information",con)  
```

#### 4) 数据库修改数据

```python
cur = con.cursor()
#新建表
cur.execute('''CREATE TABLE stocks
               (date text, trans text, symbol text, qty real, price real)''')
#插入一行新数据
cur.execute("INSERT INTO stocks VALUES ('2006-01-05','BUY','RHAT',100,35.14)")
#保存更改
con.commit()
#如果我们完成了连接，应该关闭它来确保任何更改都已提交，否则它们将丢失
con.close()
```

