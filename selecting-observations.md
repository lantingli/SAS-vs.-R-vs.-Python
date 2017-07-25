# R

### 查询变量的类型

常用的查询变量类型的函数有: mode, storage.mode, class 和 typeof. 这些函数有一些差别.

* storage.mode 是数据实际在内存中存储所采用的方式.
* class 是面向对象的 R, 比如说 data.frame 实际上的存储方式 \(storage.mode\) 是 list, 但是为了更好处理表单数据, 就包装成为了 data.frame 类型.
* mode 和 typeof 给出的结果很接近, 是实际上的类型, 但是在 mode 中, "integer" 和 "double" 都被认为是 "numeric".

如果需要知道环境中每个基本类型能存储的大小, 可以查询 `.Machine` 这个 list, 相应的大小存储在该 list 中.

### NA

由于种种原因, 数据中可能会出现缺失值的情况, R 会用 NA 来替代相应的空值. 如果是计算后产生的空值, 则会用 Inf 或者 NaN 来代替. 处理空值是数据分析中的必要部分.

* 判断 NA 可以使用 is.na 或者 is.nan 函数, 其中 is.na 会把 NA, Inf, NaN 都认为是 NA, 而 is.nan 则只关注 NaN.
* mean, var, sum, min, max, 等函数都有 na.rm 参数, 设置成真, 则会在计算的时候把 NA 给除去.
* lm, glm, gam 等函数有 na.action 参数, 该参数接受函数作为变量, 如 na.omit, na.fail. na.pass, na.exculde 等.
* na.omit 和 complete.cases 都可以返回一个只包含完整数据行的 data.frame, 也就是说如果一行中有一个或多个 NA, 该行就会被剔除.
* 对于 read.table 等函数可以使用 na.strings 可以把特定的数值或字符认为是 NA.

* **Selecting variables**

* |  | R | PYTHON | S |
  | --- | --- | --- | --- |
  | **By index number** |  |  |  |
  | **by Column name** |  |  |  |
  | **uUsing logic** |  |  |  |
  | **By string search** |  |  |  |
  | **Using $notation** |  |  |  |
  | **bBy simple name** |  |  |  |
  | _The attach function_ |  |  |  |
  | _The with function_ |  |  |  |
  | _Using short variable names in formulas_ |  |  |  |
  | **With the subset function** |  |  |  |
  | **bBy list subscript** |  |  |  |
* by index number

summary \(mydata\[ , 3\]\) = summary \(mydata \[3\]\)

summary\(mydata \[ c\(3,4,5,6\)\]\) = summary \(mydata\[3:6\]\)

summary\(mydata\[-\(3:6\)\]\) : exclude the columns from 3 to 6

generate a numbered list of variable names:

data.frame\(names\(mydata\)\)

summary \(mydata\[1: ncol\(mydata\)\]\)

1. by column name

   First check the name of the dataset: names\(mydata\)

   Summary\(mydata, \[“q1”\]\)

   Summary\(mydata \[ c\(“q1”, “q2”, “q3”, “q4”\)\]

   myQnames &lt;- paste \(“q”, 1:4, sep = “”\)

   summary\(mydata \[myQnames\]\)

2. Using logic

   Summary \(mydata\[c\(FALSE, FALSE, TRUE, FALSE, FALSE, FALSE\)\]

   Summary \(mydata\[as.logical\(c\(0,0,1,0,0,0\)\)\]

   Summary \(mydata \[names\(mydata\)== “q1”\]\)

   Summary \(mydata \[!names\(mydata\)==”q1”\]\)

   myQtf &lt;- names\(mydata\) == “q1” \| \(or\)

   names\(mydata\)== “q2”\|

   names\(mydata\)== “q3”\|

   names\(mydata\)== “q4”

   myQtf &lt;- names\(mydata\) %in% c\(“q1”, “q2”, “q3”, “q4”\)

   Summary\(mydata\[myQtf\]\)

3. by string search

   myQindices &lt;- grep\(“^q”, names\(mydata\)， value = FALSE\)

myQindices &lt;- grep\(“^q\[1-9\]”, names\(mydata\)， value = FALSE\)

“^q” means “find strings that begin with lowercase p”

Summary \(mydata \[myQindices\]\)

myQtf &lt;- names \(mydata\) %in% myQnames

Summary\(mydata\[myQtf\]\)

1. using $notation

Summary \(mydata $q1\)

Summary \(data.frame\(mydata$q1, mydata $q2\)\)

Summary \(c\(mydata$q1, mydata$q2\)\) not good!

1. By simple name

2. Use attach function

Attach\(myadta\)

Summary\(q1\) or Summary \(data.frame \(q1, q2, q3, q4\)

1. Use with function

With \(mydata,

Summary \(data.frame \(q1, q2, q3, q4\)\)

1. Using short variable names in formulas

Lm\(mydata $q4~ mydata $q1 + mydata $q2 + mydata $q3\)

Lm\(q4 ~ q1 +q2 +q3, data = mydata\)

1. With subset function

Summary \(subset\(mydata, select = q1 :q4\)

Summary \(

subset\(mydata, select = c\(workshop, q1:q4\)\)

\)

It is interesting to note that when using the c function within the subset function’s select argument, it combines the variable names, not the vectors themselves. So ：

Summary \(

Subset \(mydata, select = c \(q1, q2\)\)

\) \# good

Summary \(c\(mydata$q1, mydata $q2\)\) \# not good

1. By list subscript

Summary \(mydata \[\[3\]\]\) : select the third variable.

Mydata \[\[3:6\]\] : wrong.

1. Generating indices A to Z from two variable names

myqA &lt;- which \(names\(mydata\) == “q1”\)

myqZ &lt;- which\(names\(mydata\) == “q4”\)

summary \(mydata\[ , myqA : myqZ\]\)

check if some variables are numeric:

is.numeric\(mydata$workshop\)

1. Saving selected variables to a new data set

Myqs &lt;- mydata \[3:6\]

Myqs &lt;- mydata \[c\(“q1”, “q2”, “q3”, “q4”\)\]

Myqs &lt;- data.frame \(mydata $q1, mydata$q2, mydata$q3, mydata$q4\)

Myqs &lt;- subset\(mydata, select = q1:q4\)

# PYTHON

# SAS



