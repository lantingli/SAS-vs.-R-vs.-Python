## 23. Reshaping variables to observations and back

### SAS:

```
\\# wide to long
proc transpose data = mylib.mydata
out = mylib.mylong;
var q1 - q4;
by id workshop gender;
run;
data mylib.mylong;
  set mylib.mylong(rename = (COL1 = value));
  time = input(substr(_name_, 2), 1.);
  drop _name_;
run;

\\# long to wide
proc transpose data = mylib.mylong;
  out= mylib.mywide prefix = q;
  by id workshop gender;
  id time;
  var value;
run;

data mylib.mywide;
  set mylib.mywide(drop = _name_);
run;
```

### R:

```
library("reshap2")
mychanges <- c(
  q1 = "time1",
  q2 = "time2",
  q3 = "time3",
  q4 = "time4")
  mydata <- rename(mydata, mychanges)
  mydata$subject <- factor(1:8)
```

\\# reshaping from wide to long

```
 library("reshap2")
  mylong <- melt(mydata)
```

\\# again, specifying arguments

```
mylong <- melt(mydata, 
    id.vars = c("subject", "workshop", "gender"),
    measure.vars = c("time1", "time2", "time3", "time4"),
    value.name = "variable")

    \\# reshaping from long to wide 
     mywide <- dcast(mylong, subject + workshop + gender ~ variable)
```

### PYTHON:

\#1. Reshaping by pivoting DataFrame objects:

Data is often stored in CSV files or databases in so-called 'stacked' or 'record' format:

df.pivot\(index = 'date', columns = 'variable', values = 'value'\)

\#2. Reshaping by stacking and unstacking

Closely related to the pivot function are the related stack and unstack functions currently available on series and DataFrame. Here are essentially what these functions do:

stack: 'pivot' a level of the \(possibly hierarchical\) column labels, returning a DataFrame with an index with a new inner-most level of row labels.

unstack: inverse operation from stack: 'pivot' a level of the \(possibly hierarchical\) row index to the column axis, producing a reshaped DataFrame with a new inner-most level of column labels.

If the columns have a MultiIndex, you can choose which level to stack. The stacked level becomes the new lowest level in a MultiIndex on the columns;  with a 'stacked' DataFrame or Series \(having a MultiIndex as the index\), the inverse operation of stack is unstack, which by default unstacks the last level. Notice that the stack and unstack methods implicitly sort the index levels involved. Hence a call to stack and then unstack will result in a sorted copy of the original DataFrame or Series.

stacked.unstack\(\)

stacked.unstack\(1\)

stacked.unstack\('second'\) \# If the indexes have names, you can use the level names instead of specifying the level members

\#3. Multiple Levels

You may also stack or unstack more than one level at a time by passing a list of levels, in which case the end result is as if each level in the list were processed individually.

df.stack\(level = \['animal', 'hair\_length'\]\)

\#4. Reshaping by Melt

The melt\(\) function is useful to massage a DataFrame into a format where one or more columns are identifier variables, while all other columns, considered measured variables, are 'unpivoted' to the row axis, leaving just two non-identifier columns, 'variable' and 'value'. The names of those columns can be customized by supplying the var_name and value\_name parameters._

cheese = pd.DataFrame\({'first': \['Join', 'Mary'\], 'last', : \['Doe', 'Bo'\],  'height' : \[5.5, 6.0\], 'weight': \[130, 150\]}\)

pd.melt\(cheese, id\_vars = \['first', 'last'\]\)

out :

```
    first   last   variable   value 
```

0     Join   Doe   height      5.5

1     Mary  Bo     height      6.0

2     Join   Doe    weight    130.0

1. Join   Bo    weight      150.0

pd.melt\(cheese, id_vars = \['first', 'last'\], var\_name = 'quantity'\)_

Get the same results with ones above

Another way to transform is to use the wide\__to_\_long panel data convenience function

\#5. Combining with stats and GroupBy

It should be no shock that combining pivot/stack/unstack with GroupBy and the basic Series and DataFrame statistical functions can produce some very expressive and fast data manipulations.

df.stack\(\) .mean\(1\). unstack\(\) == df.groupby\(level =1, axis = 1\).mean\(\)

df.stack\(\).groupby\(level = 1\).mean\(\)

df.mean\(\).unstack\(0\)

\#6. Pivot tables

The function pandas.pivot\_table can be used to create spreadsheet-style pivot tables

pd.pivot\_table\(df, values = 'D', index = \['A', 'B'\], columns = \['C'\]\)

\#7. Adding margins

If you pass margins = True to pivot\_table, special All columns and rows will be added with partial group aggregates across the categories on the rows and columns .

df.pivot\_table\(index = \['A', 'B'\], columns = 'C', margins = True, aggfunc = np.std\)

\#8. Cross tabulations

Use the crosstab function to compute a cross-tabulation of two factors. By default crosstab computes a frequency table of the factors unless an array of values and an aggregation function are passed.

df = pd.DataFrame\({'A': \[1,2,2,2,2\], 'B': \[3,3,4,4,4\], 'C': \[1.1, NP.NAN, 1, 1\]}\)

pd.crosstab\(df.A, df.B\)

$9. Normalization

Frequency tables can also be normalized to show percentages rather than counts using the normalize argument:

pd.crosstab\(df.A, df.B, normalize = True\)

or

pd.crosstab\(df.A, df.B, normalize = 'columns'\)

or

pd.crosstab\(df.A, df.B, values = df.C, aggfunc = np.sum, normalize = True, margins = True\)

\#10. Computing indicator /dummy variables

To convert a categorical variable into a 'dummy' or 'indicator' DataFrame, fro example a column in a DataFrame \( a series\) which has k distinct values, can derive a DataFrame containing k columns of 1s and 0s.

df = pd.DataFrame\({'key': list\('bbacab'\), 'data1', : range\(6\)}\)

pd.get\_dummies\(df\['key'\]\)

or

dummies = pd.get\_dummies \(df\['key'\], prefix = 'key'\)

This function is often used along with discretization functions like cut:

values  = np.random.randn\(10\)

bins  = \[0, 0.2, 0.4, 0.6, 0.8, 1\]

pd.get\_dummies\(pd.cut\(values, bins\)\)

get\_dummies\(\) also accepts a DataFrame. By default all categorical variables\(categorical in the statistical sense, those with object or categorical dtype\) are encoded as dummy variables.

df = pd.DataFrame \({'A': \['a', 'b', 'a'\], 'B': \['c', 'c', 'b'\], 'C': \[1,2,3\]}\)

pd.get\_dummies\(df\)

out:

```
  C  A\__a  A\_b B\_b B\_c_
```

0    1  1       0      0     1

1    2  0       0      0     1

2    3  1       0      1     0

or you can control the columns that are encoded with the columns keyword.

pd.get\_dummies\(df, columns = \['A'\]\)

\#11. Factorizing values

To encode 1-d values as an enumerated type use factorize:

### 

### 

### 24. sorting data frames

### SAS :

```
proc sort data = mylib.mydata;
  by workshop;
run;

proc sort data = mylib.mydata;
  by descending gender workshop;
  run;
```

### PYTHON:

`obj = series(range(4), index = ['d', 'a', 'b', 'c'])        
obj.sort_index ()`

`with a dataframe, you can sort by index on either axis:`

`frame = dataframe(np.arrange(8). reshape((2, 4)), index = ['three', 'one'], columns = ['d', 'a', 'b', 'c'])`

`frame.sort_index()`

`frame.sort_index(axis = 1)`

`frame.sort_index(axis = 1, ascending = False)`

\\# To sort a series by its values, use its order method:  
`in : obj = series(4,7,-3, 2])        
 obj.order()`

\\# any missing values are sorted to the end of the series by default

`in: obj = series([4, np.nan, 7, np.nan, -3, 2])        
obj.order()`

\\# on dataframe, you may want to sort by the values in one or more columns. to do so, pass one or more column names to the by options:

`in : frame = dataframe({'b': [4,7,-3, 2], 'a': [0, 1, 0, 1]})`

`frame.sort_index(by  ='b')        
frame.sort_index (by = 'a', 'b'])`

### R:

```
  \\# show first four observations in order
    mydata[c(1,2,3,4), ]
  \\# show them in reverse order
    mydata [c(4,3,2,1),]
  \\# create order variable for workshop
    myW <- order(mydata$gender, mydata$workshop)
  \\# creating order variable for descending (-) workshop then gender
    myWdG <- order(-mydata$workshop, mydata$gender)
  \\# print data in WG order
  mydata[myWdG, ]
  \\# save data in WdG order
    mydataSorted <- mydata[myWdG, ]
```

## 25. Converting data structure

## 26. Character string manipulations

### SAS:

```
data mylib.giants;
  infile '\myRfolder\giants.txt'
  MISSOVER DSD LRECL = 32767
  INPUT name $char 14. @16 born mmddyy10. @27 died yymmdd10.;
  format born mmddyy10. died yymmdd10.;
  myVarlength = length(name)
  born= strip(born)

  data mylib.giants;
    set mylib.giants;
   mylower = lowcase(name);
   myupper = upcase(name)
   myproper = propcase(name)
 run;

 data mylib.giants;
   set mylib.giants;
   myFirst5 = substr(name, 1 ,5)
     \# split names using substr;
       myblank = find(name, " ");
       myfirst = strip(substr(name, 1, myBlank));
       mylast = strip(substr(name, myBlank));
     \# splip name using scan;
       myfirst = scan(name, 1, " ");
       mylast = scan(name, 2, " ");
       myfirst = tranwrd(myfirst, "R.A.", "Ronald A.");
       length mylastfirst $ 17;
       mylastfirst = strip(mylast)||","||strip(myfirst);
         or call CATX(",", mylastfirst, mylast, myfirst);

      data tukey;
        set mylib.giants;
          where mylast = "Tukey";
      run;

      data tukey;
        set mylib.giants;
          where find(mylast, "key");
      run;

      data mysubset;
        set mylib.giants;
        where mylast in ("Box", "Bayes", "Fisher", "Tukey");
      run;

      data fishorkey;
        set mylib.giants;
        if find(mylast, "Box") |
           find(mylast, "Bayes") |
           find(mylast, "Fish")|
           find(mylast, "key");
           run;

       data ArthruM;
         set mylib.giants;
           firstletter = substr(mylast, 1,1);
           if "A" <= firstletter <= "M";
           run;
```

### PYTHON：

#### 1. split a commma-separated string into a broken pieces

`val = 'a,b, guido'        
 val.split(,)`

`out : ['a', 'b', 'guido']`

`split is often combined with strip to trim whitespace(including newlines)`

`pieces = [x.strip() for x in val.split(',')`

`out : ['a', 'b', 'guido']`

#### 2. join together

`in : first + '::' + second '::' + third        
  out : 'a::b::guido'`

#### 3. detect a substring, through index and find

`in : 'guido' in val        
 val.index(,)        
 out : 1        
 val.find(':')        
 out : -1`

note the difference between find and index is that index raises an exception if the string isn't found\(versus returning -1\)

#### 4. relatively, count returns the number of occurrences of a particular substring

`in: val.count(',')      
  out : 2`

#### 5. replace will substitute occurrences of one pattern for another. this is commonly used to delete patterns, too, by passing an empty string:

`in: val.replace(',', '::')      
 out: 'a::b:: guido'`

`in: val.place(',', '')      
 out: 'ab guido'`

#### 6. python built-in string methods

count: return the number of non-overlapping occurrences of substring in the string  
 endwith, startwith: returns true if sting ends with suffix  
 join: use string as delimiter for concatenating a sequence of other strings  
 index: return position of first character in substring if found in the string. raises valueError if not found.  
 find: return position of first character of first occurrence of substring in the string. like index, but return -1 if not found  
 rfind: return position of first character of last occurrence of substring in the string, returens -1 if not found  
 replace: replace occurrences of string with another string  
 strip,rstrip, lstrip: trim whitespace, including newlines;

split: break string into list of substrings using passed delimiter  
 lower, upper  
 ljust, rjust: left justify or right justify, respectively. pad opposite side of string with spaces to return a string a minimum width.

### R:

```
gender <- c("m", “f", "m", NA, "m", "f", "m", "f")
options(width = 58)
library("stringr")
myVars <- str_c("Var", LETTERS[1:6])
```

```
setwd("c:/myRfolder")
giants <- read.fwf(
  file  = "giants.txt",
  width = c(15, 11, 11),
  col.names = c("name", "born", "died"),
  colClasses = c("character", "character", "POSIXct")

  str_length(giants$name)
  giants[giants$name =="R.A. Fisher", ]
  giants[giants$name == "R.A. Fisher   ", ]
  giant$name <- str_trim(giants$name)
  attach(giants)
  str_length(name)
```

```
  toupper(name)
  tolower(name)
  library("ggplot2")
  firstupper(tolower(name))
  str_sub(name, 1, 5)
  myNamesMatrix <- str_split_fixed(name, " ", 2)
  myfirst <- myNamesMatrix [ ,1 ]
  myLast <- myNamesMatrix [, 2]
  myFirst <- str_replace_all(myfirst, "R.A.", "Ronald A.")
  mylastFirst <- str_c(mylast, ",", "myfirst)
```

```
  myobs <myLast =="Tukey"
  myObs <- which(myLast == "Tukey")
  giants[myObs, ]
```

`myObs <- str_detect(myLast, "key")`

```
 myTable<- c("Box", "Bayes", "Fisher", "Tukey")
  myObs <- mylast %in% myTable
```

`myObs <- str_detect(mylast, "Box|Bayes|Fish|key")`

`myAthruM <- str_detect(myLastFirst, "^[A-M]")`

## 27. Dates and Times

#### 1. Calculating durations

##### SAS:

`Infile '\myRfolder\giants.txt'      
 MISSOVER DSD LRECL = 32767      
 input name $char14. @16 born mmddyy10. @27 died mmddyy10.;      
 proc print;      
 run;`

`proc print;      
 format died born mmddyy10.;      
 run;`

`data mylib.giants;      
   set mylib.giants;      
    age  = (died - born)/365.2425;      
    longAgo = (today() - died)/365.2425;      
 run;`

`proc print;      
 format died born mmddyy10. age longAgo 5.2;      
 run;`

##### PYTHON:

##### R:

`giants<- read.fwf(      
   file = "giants.txt",      
   width = c(15,11, 11)      
   col.names = c("name", "born", "died")      
   colClasses = c("character", "character", "POSIXct"),      
   row.names = "name",      
   strip.white = TRUE;      
 )`

`library("lubridate")      
 giants$born <- mdy(giants$born)`

`as.POSIXct(      
 c(-2520460800, -3558556800, -2207952000, -1721347200, -2952201600),      
 origin = "1960-01-01, tz = "UTC")`

`age<- difftime(died, born, units = "secs")      
 age <- difftime(died, born) # default age in days`

`giants$age <- round(as.numeric(age/365.2425), 2)`

`difftime(now(), died)/365.2425`

#### 2. adding durations to date-time variables

##### SAS:

`data mylib.giants;      
  set mylib.giants;      
   died  = born +age;      
   run;`

##### PYTHON:

##### R:

`age <- as.duration(      
 c(2286057600, 2495664000, 2485382400, 2685916800, 1935705600)      
 )`

#### 3. accessing date-time elements

##### SAS:

`data mylib.giants;      
  set mylib.giants;      
  myYear = YEAR(born);      
  myMonth =MONTH(born);      
  myDay = DAY(born);`

##### PYTHON:

##### R:

`year(born)      
month(born)      
day(born) # day of month      
wday(born) # day of week`

### 4.  creating date-time variables from elements

#### SAS:

`data mylib.giants;      
  set mylib.giants;      
  born = MDY(myMonth, myDay, myYear);      
run;`

#### PYTHON:

In working with time series data, we will frequently seek to:

1\) generate sequences of fixed-frequency dates and time spans

2\) conform or convert time series to a particular frequency

3\) compute 'relative' dates based on various non-standard time increments, or 'roll' dates forward or backward

Overview

Following table shows the type of time-related classes pandas can handle and how to create them. 

| Class | Remarks | How to create |
| :--- | :--- | :--- |
| Timestamp | Represents a single time stamp | to\_datetime, Timestamp |
| DatetimeIndex | Index of Timestamp | to\_datetime, date\_range, DatetimeIndee |
| Period | Represents a single time span | Period |
| PeriodIndex | Index of Period | period\_range, periodIndex |

\#1. Time Stamps vs. Time Spans

Time-stamped data is the most basic type of timeseries data that associates values with points in time. For pandas objects it means using the point in time. 

pd.Timestamp\(datetime\(2012, 5, 1\)  -&gt; Timestamp\('2012-05-01 00:00:00'\)

pd.Timestamp\('2012-05-01'\)

pd.Timestamp\(2012,5,1\)

However, in many cases it is more natural to associate things like change variables with a time span instead. The span represented by period can be specified explicitly, or inferred from datetime string format.

pd.Period\('2011-01'\)  -&gt; Period\('2011-01', M\)

pd.Period\('2012--05', freq = 'D'\)  -&gt; Period \('2012-05-01', 'D'\)

\#2. Converting to Timestamps

To convert a Series or list-like object of date-like objects e.g. strings, epochs, or a mixture, you can use the to\_datetime function. When passed a Series, this returns a Series , while a list-like is converted to a DatetimeIndex:

pd.to\_datetime\(pd.Series\(\['Jul 31, 2009', '2010-01-10', none\]\)\)

pd.to\_datetime\(\['2005/11/23', '2010.12.31'\]\)

If you use dates which start with the day first\(i.e. European style\), you can pass the dayfirst flag:

pd.to\_datetime\(\['04-01-2012 10:00'\], dayfirst = True\)

if you pass a single string to to_datetime, it returns single Timestamp. Also, Timestamp can accept the string input. note that Timestamp doesn't accept string parsing option like dayfirst or format, use to_\_datetime if these are required.

pd.to\_datetime\('2010/11/12'\)  -&gt; Timestamp\('2010-11-12 00:00:00'\)

pd.Timestamp\('2010/11/12'\)  -&gt; Timestamp\('2010-11-12 00:00:00\)

You can also pass a DataFrame of integer or string columns to assemble into a series of Timestamps.

df = pd.DataFrame\({'year': \[2015, 2016\], 'month': \[2,3\], 'day': \[4,5\], 'hour': \[2,3\]}\)

pd.to\_datetime\(df\)

pd.to\_datetime looks for standard designations of the datetime component in the column names, including :

required: year, month, day

optional: hour, minute, second, millsecond, microsecond, nanosecond

\#3. Invalid data

in version 0.17.0, the default for to\_datetime is now errors = 'raise', rather than errors = 'ignore'. this means that invalid parsing will raise rather than return the original input as in previous versions. 

Pass errors = 'coerce' to convert invalid data to NaT:

pd.to\_datetime\(\['2009/07/31', 'asd'\], errors = 'coerce'\)

\#4. Epoch Timestamps

it is also possible to convert integer or float epoch times. the default unit for these is nanoseconds \(since these are how timestamps are stored\). However, often epochs are stored in another unit which can be specified:

pd.to\_datetime\(\[1349720105, 1349806505, 1349892905, 1349979305, 1350065705\], unit = 's'\)

\#5. Generating ranges of timestamps

To generate an index with time stamps, you can use either the DatetimeIndex or index constructor and pass in a list of datetime objects:

dates = \[datetime\(2012, 5,1\), datetime\(2012, 5,2\), datetime\(2012, 5, 3\)\]

   \# note the frequency information

        index = pd.DatetimeIndex\(dates\)

   \# Automatically converted to DatetimeIndex

        index = pd.Index\(dates\)

Practically, this becomes very cumbersome because we often need a very long index with a large number of timestamps. if we need timestamps on a regular frequency, we can use the pandas functions date\__range and bdate_\_range to create timestamp indexes. 

index = pd.date\_range\('2001-1-1', periods = 1000, freq = 'M'\)

or

start = datetime\(2011, 1,1\)

end = datetime\(2012, 1,1\)

rng = pd.date\_range\(start, end\)

















#### R:

`myYear <- year(died)      
myMonth <- month(died)      
myDay <- day(died)`

`myDateString <<- paste(myYear, myMonth, myDay, SEP = "/")      
died2 <- ymd(myDateString)`

#### 5. logical comparisons with date-time variables

##### SAS:

`data born1900s;      
  set mylib.giants;      
   if born > "01jan1900"d;      
 run;`

##### R:

`giants[born >mdy("1/1/1900"), ]`

#### 6. formatting date-time output

`proc format;      
picture myFormatI      
LOW-HIGH = '%B %d, %Y is day %j of %Y'      
(DATATYPE = DATE)      
RUN;`

`proc print data = mylib.giants;      
  var born;      
  format born myFormatI40.;      
run;`

`data null;      
  set mylib.giants;      
   put name $char14. born myFormatII34.;      
 run;`

#### 7. two-digit years

#### 8. date-time conclusion

## 28. Loops

## 29. Functions



