## 16. Stacking/Concatenating/Adding data sets

### R:

```
females <- mydata[which(gender =="f"), ]
males <- mydata[which(gender =="m"), ]
both <- rbind(females, males)
```

use plyr rbind.fill

```
library("plyr")
both <- rbind.fill(females, males)
```

!!! rbind requires two sets has the same variables.

### PYTHON:

### SAS:

```
 data males;
   set mydata;
     where gender = "m";
 run;

 data females;
   set mydata;
    where gender = "f";
    run;

data both;
  set males females;
run;
```

## 17. Joining/Merging datasets

By default, SAS keep all records regardless of whether or not they match. for observations that do not have matches in the other file, the merge function will fill them in with missing values. R take the opposite approach, keeping only those that have a record in both. to get merge to keep all records, use the argument all = TRUE. you can also use all.x = TURE to keep all record in the first file regardless of whether or not they have matches in the record. the all.y = TRUE argument does the reverse.

### R:

```
mydata <- read.table("mydata.csv", header = TURE, sep = ",", na.strings = " ")
myleft <- mydata[c("id", "workshop", "gender", "q1", "q2")]
myright <- mydata[c("id", "workshop", "q3", "q4")]
both <- merge(myleft, myright, by = "id")

both <- merge(myleft, myright, by.x= "id", by.y = "id")

both <- merge(myleft, myright, by = c("id", "workshop"))

both <- merge(myleft, myright, by.x= c("id", "workshop"), by.y = c("id", "workshop"))
```

### PYTHON

```
df1 = dataframe({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1' : range(7)})
df2 = dataframe({'key': ['a', 'b', 'd'], 'data2': range(3)})

pd.merge(df1, df2, on = 'key')
```

\# if the column names are different in each object, you can specify them seperately:

```
df3 = dataframe({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df4 = dataframe(({[rkey': ['a', 'b', 'd'], 'data2': range(3)})

pd.merge(df3, df4, left_on = 'lkey', right_on = 'rkey')
```

\# in the situation above, the 'c' and 'd' values and associated data are missing from the result. by default merge does an 'inner' join; the keys in the result are the intersection. other possible options are 'left', 'right', and 'outer'. the outer join takes the union of the keys, combining the effect of applying both left and right joins.

```
pd.merge(df1, df2, how = 'outer')
```

\# many to many merges have well-defined though not necessarily intuitive behavior

```
df1 = dataframe('key': ['b', 'b', 'a', 'c', 'a', 'b'], 'data1': range(6))})
df2 = dataframe('key': ['a', 'b', 'a', 'b', 'd'], 'data2' : range(5)})

pd.merge(df1, df2, on = 'key', how = 'left')

or pd.merge(left, right, on = ['key1', 'key2', how = outer')

or pd.merge(left, right, on = 'key1', suffixes = ('_left', '_right')) # used for overlapping column names
```

\# Merging on index  
what is difference between merging on variables and merging on index?

Pandas provides facilities for easily combining together series, dataframe, and panel objects with various kinds of set logic for the indexes and relational algebra functionality in the case of join/merge-type opertaions

\#1 concatenating objects

The concat function does all of the heavy lifting of performing concatenation operations along an axis while performing optional set logic\(union or intersection\) of the indexes or the other axes. pandas.concat takes a list or dict of homogeneously -typed objects and concatenates them with some configurable handling of "what to do with the other axes":

pd.concat\(objs, axis = 0, join = 'outer', join_axes = None, ignore\_index = False, keys = None, levels = None, names = None, verify\_integrity  = False, copy = True\)_

suppose we want to associate specific keys with each of the pieces of the chopped up dataframe. we can do this using the keys argument:

result = pd.concat\(frames, keys = \['x', 'y', 'z'\]\)

\#1. set logic on the other axes

when gluing together multiple DataFrames\( or Panels \) , you have a choice of how to handle the other axes\(other than the one being concatenated\). This can be done in three ways:

1\) Take the \(sorted\) union of them all, join = 'outer'. this is the default option as it results in zero information loss.

2\) Take the intersection, join = 'inner'.

3\) Use a specific index\(in the case of dataframe\) or indexes , i.e. the join\_axes argument

\#2. Concatenating using append

A useful shortcut to concat are the append instance methods on Series and DataFrame.

result = df1.append\(df2\)

result = df1.append\(\[df2, df3\]\)

\#3. Ignoring indexes on the concatenation axis

For dataframes which don't have a meaningful index, you may wish to append them and ignore the fact that they may have overlapping indexe, to do this, use the ignore\_index argument:

result = pd.concat \(\[df1, df4\], ignore\_index = True\)

This is also a valid argument to dataframe.append:

result = df1.append\(df4, ignore\_index = True\)

\#4. Concatenating with mixed ndims

you  can concatenate a mix of Series and DataFrames. The Series will be transformed to DataFrames with the column name as the name of the Series.

s1 = pd.Series\(\['X0', 'X1', 'X2', 'X3'\], name = 'X'\)

result = pd.concat \(\[df1, s1\], axis = 1\)

result = pd.concat\(\[df1, s1\], axis = 1, ignore\_\_index = True\) \# Passin\_g ignore\_index = True will drop all name references.

\#5. More concatenating with group keys

A fairly common use of the keys argument is to override the column names when creating a new DataFrame based on existed Series. Notice how the default behaviour consists on letting the resulting DataFrame inherits the parent Series' name, when these existed.

s3 = pd.series\(\[0,1,2,3\], name = 'foo'\)

s4 = pd.series\(\[0,1,2,3\]\)

s5 = pd.series\(\[0.1.4.5\]\)

pd.concat\(\[s3, s4, s5\]. axis = 1\)

Through the keys argument we can override the existing column names

pd.concat\(\[s3, s4, s5\], axis = 1, keys = \['red', 'blue', 'yellow'\]\)

\#6. Appending rows to a Dataframe

While not especially efficient \(since a new object must be created\), you can append a single row to a DataFrame by passing a series or dict to append, which returns a new DataFrame.

s2 = pd.series\(\['X0', 'X1', 'X2', 'X3'\], INDEX = \['A', 'B', 'C', 'D'\]\)

result = df1.append\(s2, ignore\_index = True\)

\#7. Database-style DataFrame joining/merging

pandas has full-featured high performance in-memory join operations idiomatically very similar to relational databases like SQL. These methods perform significantly better than other open source implementations \(like base::merge.data.frame in R\). The reason for this is careful algorithmic design and internal layout of the data in Dataframe.

Pandas provides a single function, merge, as the entry point for all standard database join operations between DataFrame objects:

pd.merge\(left, right, how = 'inner', on = None, left_on = None, right\_on= None, left\_index = False, right\_index = False, sort = True, suffixes = '\_x', '\_y'\), copy = True, Indicator = False\)_

The how argument to merge specifies how to determine which keys are to be included in the resulting table. if a key combination does not appear in either the left or right tables, the values in the joined table will be NA. Here is a summary of the how options and their SQL equivalent names:

| Merge method | SQL Join Name | Description |
| :--- | :--- | :--- |
| left | LEFT OUTER JOIN | Use keys from left frame only |
| right | RIGHT OUTER JOIN | Use keys from right frame only |
| outer | FULL OUTER JOIN | Use union of keys from both frames |
| inner | INNER JOIN | Use intersection of keys from both frames |

result = pd.merge\(left, right, how = 'left', on = \['key1', 'key2'\]\)

### SAS

```
 data mylib.myleft;
   set mylib.mydata;
     keep id workshop gender q1 q2;
   proc sort ;
     by id workshop;
 run;

data mylib.myright;
  set mylib.mydata;
    keep id workshop q3 q4;
    proc sort;
      by id workshop;
  run;

  data mylib.both;
    merge mylib.myleft mylib.myright;
    by id workshop;
  run;
```

## 18. Creating summarized or aggregated data sets

## R:

\# the aggregate function  
 \# mean by workshop and gender  
 myagg1 &lt;- aggregate \(q1, by = data.frame\(workshop, gender\), mean, na.rm = TRUE\)

a. the tapply function

`myagg2 <- tapply(q1, data.frame(workshop, gender), mean, na.rm = TRUE)`

b. tabular aggregation

`table of counts`

`table(workshop)`

`table(gender, workshop)`

`mycounts <- table(gender, workshop)`

`mode(mycounts)`

`class(mycounts)`

counts in summary/aggregate stype

`mycountsDF <- as.data.frame(myCounts)`

clean up

`mydata["Zq1"] <- NULL`

`rm(myAgg1, myAGG2)`

the plyr and reshape2 packages

### PYTHON:

another kind of data combination operation is alternatively referred to as concatenation, binding, or stacking. Numpy has a concatenate function for doing this with raw Numpy arrays:

```
     arr = np.arrange(12). reshape((3,4))
     np.concatenate([arr, arr], axis =1)
```

the concat function in pandas provides a consistent way to address each of these concerns.

```
 s1 = series([0,1], index = [;a', 'b')
 s2 = series([2,3,4], index = ['c', 'd', 'e'])
 s3 = series([5,6], index = 'f', 'g'])
 pd.concat([s1, s2, s3]) \# by default concat works along axis = 0, producing another series. if you pass axis = 1, the result will instead be a dataframe(axis = 1 is the columns)
```

Group by: split -apply-combine

by 'group by' we are referring to a process involving one or more of the following steps:

-- splitting the data into groups based on some criteria

-- applying a function to each group independently

-- combining the results into a data structure

Of these, the split step is the most straightforward. In fact, in many situations you may wish to split the data set into groups and do something with those groups yourself. in the apply step, we might wish to one of the following:

Aggregation: computing a summary statistic \( or statistics\) about each group, Some examples:

-- Compute group sums or means

-- Compute group sizes/counts

Transformation: perform some group-specific computations and return a like-indexed. Some examples:

---Standardizing data\(zscore\) within group

-- Filling NAs within groups with a value derived from each group

Filtration: discard some groups, according to a group-wise computation that evaluates True or False. some examples:

-- Discarding data that belongs to groups with only a few members

--Filtering out data based on the group sum or mean

Some combination of the above: Groupby will examine the results of the apply step and try to return a sensibly combined result if it doesn't fit into either of the above two categories.

\#1. Splitting an object into groups

pandas objects can be split on any of their axes. The abstract definition of grouping is to provide a mapping of labels to group names. To create a groupby object, you can do the following:

grouped =obj.groupby\(key\)

grouped = obj.groupby\(key, axis =1\)

grouped = obj.groupby\(\[key1, key2\]\)

grouped = df.groupby\('A'\)

grouped = df.groupby\(\['A', 'B'\]\)

\#2. Groupby sorting

By default the group keys are sorted during the groupby operation. you may however pass sort = False for potential speedups.

df2 = pd.DataFrame\({'X': \['B', 'B', 'A', 'A'\], 'Y': \[1,2,3,4\]}\)

DF2.GROUPBY\(\['X'\]\).SUM\(\)

df3.groupby\(\['X'\]\).get\_group\('B'\)

\#3. DataFrame column selection in GroupBy

Once you have created the GroupBy object from a DataFrame, you might want to do something different for each of the columns. thus, using \[ \] similar to getting a column from a DataFrame, you can do:

grouped = df.groupby\(\['A'\]\)

grouped\_C = grouped\['C'\]

grouped\_D = grouped\['D'\]

This is mainly syntactic sugar for the alternative and much more verbose:

df\['C'\].groupby\(df\['A'\]\)

\#4. Selecting a group

A single group can be selected using GroupBy.get\_group\( \)

grouped.get\_group\('bar'\)

Or for an object grouped on multiple columns

df.groupby\(\['A', 'B'\]\).get\_group\(\('bar', 'one'\)\)

\#5. Aggregation

Once the GroupBy object has been created, several methods are available to perform a computation on the grouped data

An obvious one is aggregation via the aggregate or equivalently agg method:

grouped = df.groupby\(\['A', 'B'\]\)

grouped.aggregate\(np.sum\)

\#6. Applying multiple functions at once

With grouped Series you can also pass a list or dict of functions to do aggregation with, outputting a DataFrame:

grouped = df.groupby\['A'\)

grouped\['C'\].agg\(\[np.sum, np.mean, np.std\]\)

If a dict is passed, the keys will be used to name the columns. Otherwise the function's name will be used.

grouped\['D'\].agg\({'result1': np.sum,

```
                          'result2': np.mean}\)
```

On a grouped DataFrame, you can pass a list of functions to apply to each column, which produces an aggregated result with a hierarchical index:

grouped.agg\(\[np.sum, np.mean, np.std\]\)

\#7. Applying different functions to DataFrame columns

By passing a dict to aggregate you can apply a different aggregation to the columns of a DataFrame

grouped.agg\({'C':np.sum,

```
                     'D': lambda x: np.std\(x, ddof =1\)}\)
```

The function names can also be strings. In order for a string to be valid it must be either implemented on GroupBy or available via  dispatching:

grouped.agg\({'C': 'sum', 'D': 'std'}\)

If you pass a dict to aggregate, the ordering of the output columns is non-deterministic. If you want to be sure the output columns will be in a specific order, you can use an OrderedDict:

grouped.agg\(OrderedDict\(\[\('D', 'std'\), \('C', 'mean'\)\]\)\)

### 

### 

### 

### 

### 

### 

### SAS:

\\# get means of q1 for each gender

```
    proc summary data = lib.mydata mean nway;
      class gemder;
      var q1;
      output out = mylib.myagg;
    run;

  data mylib.myag;
    set mylib.myagg;
    where _stat_ = 'MEAN';
    keep gender q1;
    rename q1 = meanQ1;
  run;
```

\\# merge aggregated data back into mydata;

```
  proc sort data = mylib.mydata;
       by workshop gender;
     run;

     proc sort data = mylib.myagg;
       by workshop gender;
     run;

     data mylib.mydata2;
       merge mylib.mydata mylib.myagg;
       by workshop gender;
     run;
```

## 19. By or Split-file processing

### SAS:

```
LIBNAME mylib 'C: \myRfolder';

proc means data = mylib.mydata;
  run;

proc sort data = mylib.mydata;
  by gender;
run;

proc means data = mylib.mydatal
  by gender;
run;

proc sort data = mylib.mydata;
  by workshop gender;
run;

proc means data = mylib.mydata;
  by  workshop gender;
run;
```

### R:

    load(file = "mydata.RData")
    attach(mydata)
    options(width = 64)
       \# get means of q variables for all observations
          mean(mydata[c("q1", "q2", "q3", "q4")], na.rm = TRUE)
       \# now get means by gender
          myBYout <- by(mydata[c("q1", "q2", "q3", "q4")], mydata["gender"], mean, na.rm = TRUE)
          mode(myBYout)
          class(myBYout)
          myBYdata <- as.data.frame((as.table(myBYout)))
       \# get range by workshop and gender
         myvars<- c("q1", "q2", "q3", "q4")
         myBys <- mydata[c("workshop", "gender")]
         myBYout <- by(mydata[myVars], myBys, range, na.rm = TRUE)
       \# converting output to data frame
         mode(myBYout)
         class(myBYout)
         names(myBYout)
         myBYout[[1]]`

\# a data frame the long way

```
 myBYdata <- data.frame(
       rbind(myBYout[[1]], myBYout[[2]], 
             myBYout[[3]], myBYout[[4]])
     )
```

\# a data frame using do.call

`myBYdata <- data.frame(do. call(rbind, myBYout))`

## 20. Removing duplicate observations

### SAS:

```
libname mylib 'C:\myRfolder';
  data mycopy;
    set mylib.mydata;
  data lasttwo;
    set mylib.mydata;
      if id get 7;
  run;

  data duplicates;
    set mycopy lasttwo;
  run;

  proc sort noduprec data = duplicates;
    by id workshop gender q1 -q4;
  run;

  proc sort nodupkey equals data= mycopy;
    by workshop gender;
  run;
```

### PYTHON:

```
data = dataframe({'k1': ['one'] *3 +['two'] *4, 
                  'k2': [1,1,2,3,3,4,4]})
```

the dataframe method duplicated returns a boolean series indicating whether each row is a duplicate or not:

```
data.duplicated() \#return true or false
```

relatively, drop\_duplicates returns a dataframe where the duplicated array is true:

```
data.drop_duplicates()
```

both of these methods by default consider all of the columns; alternatively you can specify any subset of the them to detect duplicates. suppose we had a addtional column of values and wanted to filter duplicates only based on the 'k1' column :

```
data['v1'] = range(7)
data.drop_duplicates(['k1'])
```

duplicated and drop\_duplicated by default keep the first observed value combination. passing take\_last = true will return the last one:

```
data.drop_duplicates(['k1', 'k2'], take_last = true)
```

### R:

`load("mydata.RData")`

\\# create some duplicates  
 `myDuplicates <- rbind (mydata, mydata[1:2, ])`

\# get rid of duplicates without seeing them

`myNoDuplicates <- unique(myDuplicates)`

\# before getting rid of them, need to check the location of duplicates

`myDuplicates <- duplicated(myDuplicates)`

\# print a report of just the duplicate records

`attach(myDuplicates)                  
   myDuplicates[DupRecs, ]`

\# Remove duplicates and duplicated variable  
   `myNoDuplicates <- myDuplicates[!DupRecs, -7]`

or according to more than one variable

`mykeys <- c("workshop", "gender")                  
   mydata$DupKeys <- duplicated(mydata[ , myKeys])`

## 21. Selecting first or last observations per group

### SAS:

```
proc sort data = sasuser.mydata;
  by workshop gender;
run;

data sasuser.mylast;
  set sasuser.mydata;
  by workshop gender;
  if last.gender;
run;
```

### R:

```
mydata$id <- row.names(mydata)
mybys <- data.frame(mydata$workshop, mydata$gender)
mylastlist <- by(mydata, myBys, tail, n = 1)
```

\# back into a data frame

```
mylastDF <- do.call(rbind, mylastlist)
```

\# another way to create the data frame\)

```
mylastDF <- rbind(mylastlist[[1]], 
                  mylastlist[[2]],
                  mylastlist[[3]],
                  mylastlist[[4]])
```

\# generating just an indicator variable

```
mylastDF$lastgender <- rep(1, nrow(mylastDF))
mylastDF2 <- mylastDF[ c("id", "lastgender")]
mydata2 <- merge(mydata, mylasDF2, by = "id", all = TRUE)
mydata2$lastgender[is.na(mydata2$lastgender)] <- 0
```

## 22. Transposing or flipping data sets

### SAS:

```
proc transpose data = mylib.mydata out = mycopy;
run;

proc transpose data = mycopy out = myFixed;
run;
```

### R:

```
myQs <- c("q1", "q2", "q3", "q4")
myQdf <- mydata[ , myQs]
myFlipped <- t(myQdf)
class(myFlipped) # coerced into a matrix!
myFixed <- as.data.frame(t(myFlipped))
```

\# again, but with all the data

```
options(width = 60)
myFlipped <- t(mydata)
myFixed <- t(myFlipped)
myFixed <- data.frame(myFixed)
str(myFixed)

myQs <- c("q1", "q2", "q3", "q4")
myFixed[ , myQs] <- lapply(myFixed[ , myQs], as.numeric
```



