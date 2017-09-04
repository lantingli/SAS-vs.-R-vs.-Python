

# Data structure

## R

数据结构是数据类型的封装方式。R的基本数据结构分为两类（如下表所示），一类是可以存放相同类型数据的\(**同质的**\)，一类是可以存放不同类型数据的\(**异质的**\)。

1. ## 数据类型

| 数值型\(numeric\) | 字符型\(character\) | 逻辑型\(logic\) | 复数型\(complex\) | 空值 |
| :--- | :--- | :--- | :--- | :--- |
| Double, integer | character | Logic | Complex | NA, NULL |
| 整型:1,2; 浮点型: 3.14 | 'ABC',  'Mary' | TRUE, FALSE, T, F | 3 + 2i | NA, NULL |

1. ## 不同的数据结构可以存储的数据类型

| 维度 | 同质的 \(Homogeneous\) | 异质的 \(Heterogeneous\) |
| --- | --- | --- |
| 1维 | Atomic vector | List |
| 2维 | Matrix | Data frame |
| 多维 | Array |  |

1. ## 数据结构

R has several different structures including vectors, factors, data frames, matrices, arrays and lists. The data frame is most like a data set in SAS. R is flexible enough to allow you to create your own data structures.

* ### a. Arrays（Vectors and matrices\)

**向量：**

R中的向量可以理解为一维的数组，每个元素的mode必须相同，可以用c\(\)进行创建，如

`x <- c(1:9)   y <- c('Mike', 2, FALSE)`

vector, 向量, 或者说是原子向量, 类似一维的 array, 存储相同的基本类型, 可以是逻辑型 \(logical\), 整型 \(integer\), 浮点型 \(numeric, double\), 字符型 \(character\) 等, 如果有字符型的元素, 则所有的值都会转为字符型。

数据类型转换遵循的顺序为: 逻辑型 &lt; 整型 &lt;浮点型 &lt; 复数型 &lt; 字符型

向量有三个特征: 一个是基本类型, 可以使用 typeof\(\) 等函数查看; 一个是长度, 可以使用 length\(\) 函数查看; 还有一个是属性, 可以使用 attributes\(\) 函数查看. 类型指的是构成向量的元素的基本类型, 长度指的是元素的个数, 而属性指的是有关向量的名称等其他方面的特征, 如可以使用 names\(\) 去查看或者设置每个元素的名称.

**矩阵：**

R中的矩阵可以理解为二维数组，每一个元素必须要有相同的mode，使用matrix进行创建，matrix的形式为：

`matrix(vector, nrow=number_of_rows, ncol=number_of_columns, byrow=logical_value, dimnames=list(rownames, colnames))`，

该函数中，vector中为矩阵的元素，nrow表示行数，ncol表示列数，byrow为一个布尔向量表示是否按照行进行填充，默认按照列填充，dimnames为可选的制定行和列的名称。

输入a\[i,\]获取矩阵a的第i行，输入a\[,j\]获取矩阵a的第j列，输入a\[i,j\]获取矩阵a的第i行第j列

**数组：**

R中的数组使用array进行创建，与向量或者矩阵不同的是，array可以是多维的。array中的数据同样是相同mode的，array函数的像是如下：

array\(vector, dimensions, dimnames\)，其中vector包含array中的元素，dimensions是一个向量指定array各个维度的大小，dimnames是一个list指定各个维度对应的名称。

* ### b. Factors

因子\(Factor\) 是一种能够节省内存空间的数据结构. 如果一系列的值中有较多重复的值或者用来分组的值, 可以使用 factor, factor 中只会存储一份原值, 而原来的值本身会存为数字, 这样就会节省空间.

原值被称为水平 \(level\), 可以设置顺序等. levels 函数可以返回 factor 所有可能的 level, 而 nlevels 则可以返回 level 的个数.

把一个向量转换成 factor 使用 as.factor 函数. 有的时候需要我们把数字先转换成 factor, 经过一定处理之后需要再把 factor 转换称为数字, 这个时候不能直接使用 as.numeric, 因为 as.numeric 会直接返回 factor 内部的值, 而不是原来的值. 我们需要先使用 as.character 或者 levels 先得到字符, 然后再转换为数字.

* ### c. Data frames

数据框是R语言里中一种重要的数据结构，其内部可以由多种数据类型，每一列是一个**变量**，每行是一个**观测记录**。数据框的每**一列数据的类型必须一致，但可以将多个类型的不同列放到一起组成数据框**。 数据框是我们常用的进行数据分析的数据存储方式，与数据库中表结构\(每一行对应一个记录，每一列对应一个字段\)类似，数据框使用data.frame\(name1=col1, name2=col2,...\)进行创建，注意是列主导。 R语言中用函数data.frame\(col1, col2, col3, ...\)创建一个数据框，其中col1、col2、col3等等可以使任何类型（字符型、数值型、逻辑型）的列向量。

```
patientdata <- data.frame(patientID, name, age, diabetes, status, stringsAsFactors = FALSE)
```

**如何查找数据框中的数据**？用符号$和数据框中的列名（列向量名称）来查找选取数据框中的某一列。

```
age1 <- patientdata$age
```

**如何选取数据框的其中一部分数据**？给数据框中指定列名来确定需要选取的数据。

```
#选取其中一部分数据
subdata <- patientdata[c("diabetes", "status")]
```

* ### d. Lists

  列表是R语言中一种复杂数据结构，是一些数据**对象**的有序集合。因此，一个列表中可能是**若干向量、矩阵、数组、数据框，甚至是其他列表的组合**。列表是R语言中能够**存储各种数据结构、包罗万象**的容器。

列表也存在长度属性， 用length\(\)函数可以获取列表的长度\(元素个数\)

R语言中用函数list\(object1, object2, object3, ...\)创建一个列表，其中object对象是R语言中的任何数据结构。此外，还可以给列表中的对象命名：list\(name1=object1, name2=object2, name3=object3, ...\)，命名主要是为了用户方便查找。

**在实际的数据分析中函数返回结果常常会涉及到各种数据结构，需要用列表存储函数返回的结果**。

R中的列表和Python中的dict很像，使用list进行创建，list的形式为list\(name1=object1, name2=object2,...\)

**列表内容的访问：**

通过**双重方括号\[\[ \]\]**中指明**代表某个成分的数字**或**名称**来获取列表中的元素。

方法一： 按照位置访问    ls\[\[1\]\] 访问第一个元素的数据

方法二： 按照元素名称访问  ls\[\['first'\]\] 访问名称为 first 的元素内容

下面的代码是采用成分的**数字**来获取列表中的元素

```
#数字1表示第1个元素--数据框type1
ls <- list('a'=1, 'b'=2, 'c'=3)
ls[[1]] 返回 1
ls[['b']] 返回 2
```

## Python\(Pandas\)

* ###  Series

Series 是一个一维数组结构的，可以存入任一一种python的数据类型\(integers, strings, floating point numbers, Python objects, etc.\)。常见的创建Series的方法是从列表或者数组创建：

```
>>>s = pd.Series(data, index=index)
>>>data = pd.Series([0.25, 0.5, 0.75, 1.0]) #通过列表创建
>>>data
0    0.25
1    0.50
2    0.75
3    1.00
dtype: float64
```

可以发现， Series包含一列值和一列索引， 我们可以通过 values 和 index 属性来访问值和索引

```
>>>data.values
array([ 0.25,  0.5 ,  0.75,  1.  ])
>>>data.index
RangeIndex(start=0, stop=4, step=1)
```

类似于Numpy中的数组， 可以通过python中常见的方括号符号来访问Series的元素值

```
>>>data[1]  # 记住， python的索引是从0开始， 与R不同
0.5
>>>data[1:3]  # 切片
1    0.50
2    0.75
dtype: float64
```

#### Series作为numpy array

通过上述的例子， 可以看到Series 可以和一位Numpy数组相互转换。 关键的区别是索引的存在：Numpy数组隐含整数的索引来访问数组元素， 而 Series 则有明定义的索引。

有了明确定义的索引， Series对象具有更多的特性。 例如， 索引不需要是整数， 可以是任何类型的值。 例如将字符串作为索引:

```
>>>data = pd.Series([0.25, 0.5, 0.75, 1.0],
                    index = ['a', 'b', 'c', 'd'])
>>>data
a    0.25
b    0.50
c    0.75
d    1.00
dtype: float64
```

那么元素可以这样访问：

```
>>>data['b']
0.5
```

甚至可以说那个非连续的索引:

```
>>>data = pd.Series([0.25, 0.5, 0.75, 1.0],
                 index=[2, 5, 3, 7])
>>>data
2    0.25
5    0.50
3    0.75
7    1.00
dtype: float64

>>>data[5]
0.5
```

#### Series作为特殊的字典

某种程度上说， Series类似python的一种特殊字典。 字典是一种将任意键映射到任意值的数据结构， 而 Series 则是将有类型的键映射到一组有类型的值。 而类型十分重要： 例如在Numpy中对应类型二编译的代码比python中常用的列表在一些操作上更加高效。 同样的， Series的类型信息使得它比普通的python字典在一些操作中更高效。

Series和字典的类似性在Series可以直接由字典创建的特性中十分明显。

```
>>>population_dict = {'California': 38332521,
                   'Texas': 26448193,
                   'New York': 19651127,
                   'Florida': 19552860,
                   'Illinois': 12882135}
>>>population = pd.Series(population_dict)
>>>population
California    38332521
Florida       19552860
Illinois      12882135
New York      19651127
Texas         26448193
dtype: int64
```

可以实现字典风格的典型操作：

```
>>>population['California']
38332521
```

而且，Series还支持数组风格的切片操作， 这是普通字典所不具有的：

```
>>>population['California':'Illinois']
California    38332521
Florida       19552860
Illinois      12882135
dtype: int64
```

### DataFrame

Pandas 中另外一个基本结构是DataFrame. 类似上述讨论的 Series， DataFrame 可以被看做Numpy 数组的一种衍生， 或者是字典的特殊形式。

#### DataFrame 作为Numpy 数组

如果Series可以被看做一个包含索引的一维数组类似物， 那么 DataFrame 可以看作具备行索引和列名的二维数组。 可以将DataFrame 看作一系列对齐的Series， 对齐意味着这些Series有共同的索引。

```
>>>area_dict = {'California': 423967, 'Texas': 695662, 'New York': 141297,
             'Florida': 170312, 'Illinois': 149995}
>>>area = pd.Series(area_dict)
>>>area
California    423967
Florida       170312
Illinois      149995
New York      141297
Texas         695662
dtype: int64
```

我们可以将该Series和上面的population Series一起来构建一个二维的对象

```
>>>states = pd.DataFrame({'population': population,
                       'area': area})
>>>states
	   area	    population
California	423967	38332521
Florida	        170312	19552860
Illinois	149995	12882135
New York	141297	19651127
Texas	        695662	26448193
```

类似Series对象， DataFrame也有索引属性， 用来访问索引标签

```
>>>states.index
Index(['California', 'Florida', 'Illinois', 'New York', 'Texas'], dtype='object')
```

另外， DataFrame还包含一个 columns 属性， 是一个包含列标签的索引对象。

```
>>>states.columns
Index(['area', 'population'], dtype='object')
```

因此， DataFrame 可以认为是一种特殊的Numpy两维数组， 行和列都有特定的索引来访问数据。

#### DataFrame 作为特殊的字典

类似的， 我们可以将DataFrame看作特殊的字典。 字典将键映射到一个值， 而DataFrame将列名映射到一个Series的数据列。例如， 访问 area 属性返回一个 Series对象， 包含了之前看到的面积。

```
>>>states['area']
California    423967
Florida       170312
Illinois      149995
New York      141297
Texas         695662
Name: area, dtype: int64
```

一个比较容易混淆的点：在二维Numpy 数组中， data\[0\] 总是会返回第一行。 对于DataFrame，data\['col0'\]则会返回第一列。 出于这个原因， 最好将DataFrame 看作是一种特殊的字典。

#### 构建DataFrame





There are  numerous ways to construct a dataframe, though one of the most common is from a dict of equal-length lists or numpy arrays

```
    3. Index Objects
```

## SAS

SAS has one main data structure

