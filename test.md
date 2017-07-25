## R

数据结构是数据类型的封装方式。R的基本数据结构分为两类（如下表所示），一类是可以存放相同类型数据的\(**同质的**\)，一类是可以存放不同类型数据的\(**异质的**\)。

1. ## 数据类型

| Numeric | Character |
| --- | --- |
| Double, integer | character, logic |

1. ## 不同的数据结构可以存储的数据类型

| 维度 | 同质的 \(Homogeneous\) | 异质的 \(Heterogeneous\) |
| --- | --- | --- |
| 1维 | Atomic vector | List |
| 2维 | Matrix | Data frame |
| 多维 | Array |  |

1. ## 数据结构包括

R has several different structures including vectors, factors, data frames, matrices, arrays and lists. The data frame is most like a data set in SAS. R is flexible enough to allow you to create your own data structures.

* ### a. Arrays（Vectors and matrices\)

**向量：**

R中的向量可以理解为一维的数组，每个元素的mode必须相同，可以用c\(x:y\)进行创建，如

`x <- c(1:9)`

vector, 向量, 或者说是原子向量, 类似一维的 array, 存储相同的基本类型, 可以是逻辑型 \(logical\), 整型 \(integer\), 浮点型 \(numeric, double\), 字符型 \(character\) 等, 如果有字符型的元素, 则所有的值都会转为字符型. 向量有三个特征: 一个是基本类型, 可以使用 typeof\(\) 等函数查看; 一个是长度, 可以使用 length\(\) 函数查看; 还有一个是属性, 可以使用 attributes\(\) 函数查看. 类型指的是构成向量的元素的基本类型, 长度指的是元素的个数, 而属性指的是有关向量的名称等其他方面的特征, 如可以使用 names\(\) 去查看或者设置每个元素的名称.

**矩阵：**

R中的矩阵可以理解为二维数组，每一个元素必须要有相同的mode，使用matrix进行创建，matrix的形式为：

`matrix(vector, nrow=number_of_rows, ncol=number_of_columns, byrow=logical_value, dimnames=list(rownames, colnames))`，

该函数中，vector中为矩阵的元素，nrow表示行数，ncol表示列数，byrow为一个布尔向量表示是否按照行为主进行填充，默认按照列为主，dimnames为可选的制定行和列的名称。

输入a\[i,\]获取矩阵a的第i行，输入a\[,j\]获取矩阵a的第j列，输入a\[i,j\]获取矩阵a的第i行第j列

**数组：**

R中的数组使用array进行创建，与向量或者矩阵不同的是，array可以是多维的。array中的数据同样是相同mode的，array函数的像是如下：

array\(vector, dimensions, dimnames\)，其中vector包含array中的元素，dimensions是一个向量指定array各个维度的大小，dimnames是一个list指定各个维度对应的名称。

### b. Factors

Factor 是一种能够帮助节省内存空间的方式. 如果一系列的值中有较多重复的值, 可以使用 factor, factor 中只会存储一份原值, 而原来的值本身会存为数字, 这样就会节省空间.

原值被称为水平 \(level\), 可以自己去设置顺序等. levels 函数可以返回一个 factor 所有可能的 level, 而 nlevels 则可以返回 level 的个数.

把一个向量转换成 factor 只要使用 as.factor 函数. 有的时候需要我们把数字先转换成 factor, 经过一定处理之后需要再把 factor 转换称为数字, 这个时候不能直接使用 as.numeric, 因为 as.numeric 会直接返回 factor 内部的值, 而不是原来的值. 我们需要先使用 as.character 或者 levels 先得到字符, 然后再转换为数字.

### c. Data frames

数据框是R语言里中的一种数据结构，其内部可以由多种数据类型，每一列是一个**变量**，每行是一个**观测记录**。数据框的每**一列数据的模式必须唯一，但可以将多个类型的不同列放到一起组成数据框**。 数据框是我们常用的进行数据分析的数据存储方式，和[**数据库**](http://lib.csdn.net/base/mysql "MySQL知识库")的每一行对应一个记录，每一列对应一个字段，数据框使用data.frame\(name1=col1, name2=col2,...\)进行创建，注意是列主导。 R语言中用函数data.frame\(col1, col2, col3, ...\)创建一个数据框，其中col1、col2、col3等等可以使任何类型（字符型、数值型、逻辑型）的列向量。

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

### d. Lists

A list is a very flexible data structure. You can use it to store combinations of any other objects, even other lists. The objects stored in a list are called its components. This is a broader term than variables, or elements of a vector, reflecting the wider range of objects possible.  列表是R语言中最为复杂的一种数据结构，是一些**对象（或成分）**的有序集合。因此，一个列表中可能是**若干向量、矩阵、数组、数据框，甚至是其他列表的组合**。列表是R语言中能够**存储各种数据结构、包罗万象**的容器。

R语言中用函数list\(object1, object2, object3, ...\)创建一个列表，其中object对象是R语言中的任何数据结构。此外，还可以给列表中的对象命名：list\(name1=object1, name2=object2, name3=object3, ...\)，命名主要是为了用户方便查找。

**在实际的数据分析中需要运用多种函数，函数返回结果调用数据的时候会涉及到各种数据结构，需要用列表存储函数返回的结果**。

R中的列表和Python中的dict很像，使用list进行创建，是行为主导的，list的形式为list\(name1=object1, name2=object2,...\)

**如何查找列表中的数据**？

通过**双重方括号**中指明**代表某个成分的数字**或**名称**来获取列表中的元素。

下面的代码是采用成分的**数字**来获取列表中的元素

```
#数字1表示第1个元素--数据框type1
listdataframe <- kpi[[1]]
```

## Python

```
   1. Series
```

Series: \(\[\[\]\] 和 {\[\]} 有什么区别？ A series is a one-dimensional array-like object containing an array of data \(of any NumPy data type\) and an associated array of data labels, called its index. 1\) The simplest series is formed from only an array of data: Obj = series\(\[4,7,-5,3\]\) Since we did not specify an index for the data, a default one consisting of the integers 0 through N-1 \(where N is the length of the data\) is created. You can get the array representation and index object of the series via its values and index attributes. 2\) Often it will be desirable to create a series with an index identifying each data point: Obj2 = series \(\[4,7,-5,3\], index = \[‘d’, ‘b’, ‘a’, ‘c’\]\) 3\) Another way to think about a series is as a fixed –length, ordered dict, as it is mapping of index values to data values. You can create a series from it by passing the dict: Sdata = {‘ohio’: 35000, ‘Texas’: 71000, ‘Oregon’: 16000, ‘Utah’ : 5000} Obj3 = series \(sdata\) When only passing a dict, the index in the resulting series will have the dict’s keys in sorted order. States = \[‘California’, ‘ohio’, ‘oregon’, ‘Tesas’\] Obj4 = series \(sdata, index = states\)

```
   2. DataFrame
```

There are  numerous ways to construct a dataframe, though one of the most common is from a dict of equal-length lists or numpy arrays

```
    3. Index Objects
```

## SAS

SAS has one main data structure

