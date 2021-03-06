# 01 Pandas是什么

Pandas 是一个开源的第三方 Python 库，从 Numpy 和 Matplotlib 的基础上构建而来，享有数据分析“三剑客之一”的盛名（NumPy、Matplotlib、Pandas）。Pandas 已经成为 Python 数据分析的必备高级工具，它的目标是成为强大、灵活、可以支持任何编程语言的数据分析工具。



![pandas教程](http://c.biancheng.net/uploads/allimg/210901/1513331Z3-0.gif)
图1：Pandas Logo


Pandas 这个名字来源于面板数据（Panel Data）与数据分析（data analysis）这两个名词的组合。在经济学中，Panel Data 是一个关于多维数据集的术语。Pandas 最初被应用于金融量化交易领域，现在它的应用领域更加广泛，涵盖了农业、工业、交通等许多行业。

Pandas 最初由 Wes McKinney（韦斯·麦金尼）于 2008 年开发，并于 2009 年实现开源。目前，Pandas 由 PyData 团队进行日常的开发和维护工作。在 2020 年 12 月，PyData 团队公布了最新的 Pandas 1.20 版本 。

在 Pandas 没有出现之前，Python 在数据分析任务中主要承担着数据采集和数据预处理的工作，但是这对数据分析的支持十分有限，并不能突出 Python 简单、易上手的特点。Pandas 的出现使得 Python 做数据分析的能力得到了大幅度提升，它主要实现了数据分析的五个重要环节：

- 加载数据
- 整理数据
- 操作数据
- 构建数据模型
- 分析数据

## Pandas主要特点

Pandas 主要包括以下几个特点：

- 它提供了一个简单、高效、带有默认标签（也可以自定义标签）的 DataFrame 对象。
- 能够快速得从不同格式的文件中加载数据（比如 Excel、CSV 、SQL文件），然后将其转换为可处理的对象；
- 能够按数据的行、列标签进行分组，并对分组后的对象执行聚合和转换操作；
- 能够很方便地实现数据归一化操作和缺失值处理；
- 能够很方便地对 DataFrame 的数据列进行增加、修改或者删除的操作；
- 能够处理不同格式的数据集，比如矩阵数据、异构数据表、时间序列等；
- 提供了多种处理数据集的方式，比如构建子集、切片、过滤、分组以及重新排序等。

上述知识点将在后续学习中为大家一一讲解。

## Pandas主要优势

与其它语言的数据分析包相比，Pandas 具有以下优势：

- Pandas 的 DataFrame 和 Series 构建了适用于数据分析的存储结构；
- Pandas 简洁的 API 能够让你专注于代码的核心层面；
- Pandas 实现了与其他库的集成，比如 Scipy、scikit-learn 和 Matplotlib；
- Pandas 官方网站（[点击访问](https://pandas.pydata.org/)）提供了完善资料支持，及其良好的社区环境。

## Pandas内置数据结构


我们知道，构建和处理二维、多维数组是一项繁琐的任务。Pandas 为解决这一问题， 在 ndarray 数组（NumPy 中的数组）的基础上构建出了两种不同的数据结构，分别是 Series（一维数据结构）DataFrame（二维数据结构）：

- Series 是带标签的一维数组，这里的标签可以理解为索引，但这个索引并不局限于整数，它也可以是字符类型，比如 a、b、c 等；
- DataFrame 是一种表格型数据结构，它既有行标签，又有列标签。


下面对上述数据结构做简单地的说明：



| 数据结构  | 维度 | 说明                                                         |
| --------- | ---- | ------------------------------------------------------------ |
| Series    | 1    | 该结构能够存储各种数据类型，比如字符数、整数、浮点数、Python 对象等，Series 用 name 和 index 属性来描述 数据值。Series 是一维数据结构，因此其维数不可以改变。 |
| DataFrame | 2    | DataFrame 是一种二维表格型数据的结构，既有行索引，也有列索引。行索引是 index，列索引是 columns。 在创建该结构时，可以指定相应的索引值。 |


由于上述数据结构的存在，使得处理多维数组数任务变的简单。在《[Pandas Series入门教程](http://c.biancheng.net/pandas/series.html)》和《[Pandas DataFrame入门教程（图解）](http://c.biancheng.net/pandas/dataframe.html)》两节中，我们会对上述数据结构做详细讲解。

注意，在 Pandas 0.25 版本后，Pamdas 废弃了 Panel 数据结构，如果感兴趣可阅读《[Pandas Panel三维数据结构](http://c.biancheng.net/pandas/panel.html)》一节。