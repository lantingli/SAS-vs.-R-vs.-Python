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

  
` in: val.count(',')  
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

#####  SAS:

  
`data mylib.giants;  
  set mylib.giants;  
   died  = born +age;  
   run;`

##### PYTHON: 

##### R:

  
` age <- as.duration(  
 c(2286057600, 2495664000, 2485382400, 2685916800, 1935705600)  
 )`

#### 3. accessing date-time elements

#####  SAS:

  
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

####  SAS:

  
`data mylib.giants;  
  set mylib.giants;  
  born = MDY(myMonth, myDay, myYear);  
run;`



#### PYTHON:

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

##  28. Loops

##  29. Functions 



