## R:

### **1. By index number**

#### **a. Select all variables by default:**

```R
         print(mydata[ ]); 
         print (mydata[, ]);
```

#### ** b. Select the 3rd variable:  **

                                                                                                                                                                                                                                                                                                                    `print(mydata [ , 3]); print(mydata[3]);`

#### **c. Select the variables q1, q2 ,q3, q4**

`print(mydata[c(3,4,5,6)]); print(mydata[3:6])`

#### **d. Exclude q1, q2 ,q3 q4**

```
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    print\(mydata\[-c\(3,4,5,6\)\]\) ;
```

`print(mydata[-(3:6)])`

#### **e. Using indices in a numeric vector:**

`myQindices <-c (3,4,5,6); print(mydata[myQindices]) ]`

#### **f. Display the indices for all variables:**

`print(data.frame(names(mydata))`

#### **g. Using ncol to find the last index: **

`print(mydata[1:ncol(mydata)])`

### **2. By Column name**

#### a. Display all variable names:

`names(mydata)`

#### b. Select one variable :

`print(mydata["q1"]); \# pass q1 as a data frame;`

`print(mydata[ , "q1"]) # pass q1 as a vector`

#### c. Select several:

`print(mydata[c("q1", "q2", "q3", "q4")  ])`

#### d. Save a list of variable names to use:

`myQnames <- c("q1", "q2", "q3", "q4") ; print (mydata[myQnames]`

#### e. Generate a list of variable names:

`myQnames <- paste("q", 1:4, sep = " ") ;`

`print(mydata[myQnames])`

### 3. By logic

#### a. Select q1 by entering TRUE/FALSE values:

`print(mydata[c(FALSE, FALSE, TRUE, FALSE, FALSE)])`

#### b. Manually create a vector to get just q1:

`print(mydata[as.logial(c(0,0,1,0,0,0))])`

#### c. Automatically create a logical vector to get just q1:

`print(mydata[names(mydata) == "q1"]) or !names(mydata) == “q1"])`

#### d. Use the OR operator, "\|" to select q1 through q4:

`myQtf <- names(mydata) == "q1"| names(mydata) == "q2"`

`print(mydata[myQtf]`

#### e. Use the %in% operator to select q1 through q4

`myQtf <- names(mydata) %in% c("q1", "q2", "q3", "q4")`

### 4. By string search

#### a. use grep to save the q variable indices:

`myQindices <- grep("^q", names(mydata), value = FALSE)`

#### b. use grep to save the q variable names :

`value = TRUE`

#### c. use %in% to create a logical vector

`yQtf <- names (mydata) %in% myQnames; print;`

### 5. By $notation

`print(mydata$q1)`

`print(data.frame(mydata$q1, mydata$q2)`

### 6.  By simple name

`using attach function`

### 7. Using "with" function

`with(mydata, summary(data.frame(q1, q2, q3, q4)))`

### 8. with subset function

`print(subset(mydata, select = q1: q4))`

`print(subset(mydata, select = c(workshop, q1:q4)`

### 9. Select variables by list subscript

`print(mydata[[3]] )`

### 10. Generate indices A to Z from two variables

`myqA <- which(names (mydata) == "q1")`

`myqZ <- which(names(mydata) =="q4")`

`print(mydata[myqA : myqZ)`

### 11. Select numeric or character variables

`is. numeric (mydata$workshop)`

`find numeric variables:`

`mynums <- sapply(mydata, is.numeric);                
 print(mydata[myNums]`

`myA <- which(names(mydata) == "gender")`

`myZ <- which(names(mydata) == "q3")`

`myRange <- 1 :`

`length(mydata) %in% myA : myZ`

`print (mydata[myNums & myRange])`

### 12. Create a new data frame of selected variables

`myqs <- mydata[3:6]`

`myqs <- mydata[ c ("q1", "q2", "q3", "q4")]`

`myqs <- data.frame (mydata$q1, mydata$q2, mydata$q3, mydata$q4)`

`myqs <- data.frame(q1 = mydata$q1, q2 = mydata$q2, q3 = mydata$q3, q4= mydata $q4)`

`myqs <- subset(mydata, select = q1 :q4)`

## PTYHON: 

The primary function of indexing are listed below:

| Object Type | Selection | Return Value Type |
| :--- | :--- | :--- |
| Series | series\[label\] | scalar value |
| DataFrame | frame\[colname\] | series corresponding to colname |
| Panel | panel\[itemname\] | DataFrame corresponding to the itemname |



Object selection has had a number of user-requested additions in order to support more explicit location based indexing. pandas now supports three types of multi-axis indexing.

.loc is primarily label based , but may also be used with a boolean array. .loc will raise KeyError when the items are not found. Allowed inputs are:

--a single label, e.g. 5 or 'a', \(note that 5 is interpreted as a label of the index. this use is not an integer position along the index\)

-- a list or array of labels\('a', 'b', 'c'\]

--a slice object with labels 'a': 'f', \(note that contrary to usual python slices, both the start and the stop are included. 

-- a boolean array

.iloc is primarily integer position based \(from 0 to length-1 of the axis\), but may also be used with a boolean array. .iloc will raise IndexError if a requested indexer is out-of -bounds, except slice indexers which allow out-of-bounds indexing. Allowed inputs are:

--an integer e.g. 5

-- a list or array of integers \[4,3,0\]

--a slice object with ints 1:7

-- a boolean array

.ix supports mixed integer and label based access. it is primarily label based, but will fall back to integer positional access unless the corresponding axis is of integer type. .ix is the most general and will support any of the inputs in .loc and .iloc. .ix also supports floating point label schemes. .ix is exceptionally useful when dealing with mixed positional and label based hierachical indexes. 

| Object Type | Indexers |
| :--- | :--- |
| Series | S.loc\[indexer\] |
| DataFrame | df.loc\[row\_indexer, column\_indexer\] |
| Panel | p.loc\[item\_indexer, major\_indexer, minor\_indexer\] |



Pandas 进行行选择一般有三种方法：

连续多行的选择用类似于python 的列表切片

n = fandango\[1:3\]

连续指定的索引选择一行或多行，使用loc\[ \] 方法

o = fandango.loc\[1\]

p = fandango.loc\[1:3\]

连续指定的位置选择一行或多行， 使用iloc 方法

loc\[ \] 与iloc \[ \] 方法之前一个巨大的差别，那就是loc\[ \] 里的参数是对应的索引值即可，所以参数可以是整数，也可是是字符串，而iloc  \[ \] 里的参数表示的是第几行的数据， 所以只能是整数

### 1. Selecting via \[ \], which slices the rows

df\['A'\] \#列

df\[0:3\] \# 行

1. Selection by label

df.loc\[dates\[0\]\]  \# getting a cross section using a label

df.loc\[:. \['A', 'B'\]\] \# selecting on a multi-axis by label

df.loc\['20130102': '20130104', \['A','B'\]\]  \# Showing label slicing, both endpoints are included.

1. Selection by position 

df.iloc\[3\]  \#select via the position of the passed integers 行

df.iloc\[3:5, 0:2\] \# by integer slices 行 and 列

df.iloc\[\[1,2,4\], \[0,2\]\]  \# 行 and 列 \# by lists of integer position locations

df.iloc\[1:3, :\]  \# for slicing rows explicitly

df.iloc \[:, 1:3\] \# for slicing columns explicitly

df.iloc\[1,1\] \# for getting a value explicitly

1. Boolean indexing

df\[df.A &gt;0\]  \# using a single column's values to select data

df\[df &gt;0\] \# A where operation for getting

Using the isin\(\) method for filtering:

df2 = df.copy\(\)

df2\['E'\] = \['one', 'one', 'two','three', 'four', 'three'\]

df2\[df2\['E'\].isin\(\['two', 'four'\]\)\]

1. Setting

Setting a new column automatically aligns the data by the indexes

s1 = pd.series\(\[1,2,3,4,5,6\], index = pd.date\_range\('20130102', periods = 6\)\)

df\['F'\] = s1

df.at\[dates\[0\], 'A'\] = 0 \# setting values by label

df.iat\[0,1\] = 0 \# settign values by position

df.loc\[:, 'D'\] = np.array\(\[5\] \* len\(df\)\)

df2 = df.copy\(\)

df2\[df2&gt;0\] = -df2  \# a where operation with setting

## SAS:



