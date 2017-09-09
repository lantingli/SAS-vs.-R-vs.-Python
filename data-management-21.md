11. Stacking/Concatenating/Adding data sets
 \# R 
 


```
females <- mydata[which(gender =="f"), ]
males <- mydata[which(gender =="m"), ]
both <- rbind(females, males)
```



\#use plyr rbind.fill


```
library("plyr")
both <- rbind.fill(females, males)
```



!!! rbind requires two sets has the same variables.
 
 \# PYTHON
 
 
 
 
 
 
 
 
 
 
 \# SAS
 


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



12.  Joining/Merging datasets

By default, SAS keep all records regardless of whether or not they match. for observations that do not have matches in the other file, the merge function will fill them in with missing values. R take the opposite approach, keeping only those that have a record in both. to get merge to keep all records, use the argument all = TRUE. you can also use all.x = TURE to keep all record in the first file regardless of whether or not they have matches in the record. the all.y = TRUE argument does the reverse.

\# R
mydata <- read.table("mydata.csv", header = TURE, sep = ",", na.strings = " ")
myleft <- mydata[c("id", "workshop", "gender", "q1", "q2")]
myright <- mydata[c("id", "workshop", "q3", "q4")]
both <- merge(myleft, myright, by = "id")

both <- merge(myleft, myright, by.x= "id", by.y = "id")

both <- merge(myleft, myright, by = c("id", "workshop"))

both <- merge(myleft, myright, by.x= c("id", "workshop"), by.y = c("id", "workshop"))

\# PYTHON

df1 = dataframe({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1' : range(7)})
df2 = dataframe({'key': ['a', 'b', 'd'], 'data2': range(3)})

pd.merge(df1, df2, on = 'key')

\# if the column names are different in each object, you can specify them seperately:
df3 = dataframe({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df4 = dataframe(({[rkey': ['a', 'b', 'd'], 'data2': range(3)})

pd.merge(df3, df4, left_on = 'lkey', right_on = 'rkey')

\# in the situation above, the 'c' and 'd' values and associated data are missing from the result. by default merge does an 'inner' join; the keys in the result are the intersection. other possible options are 'left', 'right', and 'outer'. the outer join takes the union of the keys, combining the effect of applying both left and right joins.
pd.merge(df1, df2, how = 'outer')

\# many to many merges have well-defined though not necessarily intuitive behavior 
df1 = dataframe('key': ['b', 'b', 'a', 'c', 'a', 'b'], 'data1': range(6))})
df2 = dataframe('key': ['a', 'b', 'a', 'b', 'd'], 'data2' : range(5)})

pd.merge(df1, df2, on = 'key', how = 'left')

or pd.merge(left, right, on = ['key1', 'key2', how = outer')

or pd.merge(left, right, on = 'key1', suffixes = ('_left', '_right')) # used for overlapping column names

\# Merging on index 
what is difference between merging on variables and merging on index?





\# SAS



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

 
13. Creating summarized or aggregated data sets
\#R

     \# the aggregate function
     \# mean by workshop and gender 
     myagg1 <- aggregate (q1, by = data.frame(workshop, gender), mean, na.rm = TRUE)
     

    \# the tapply function
     myagg2 <- tapply(q1, data.frame(workshop, gender), mean, na.rm = TRUE)
     
    \# tabular aggregation
     \#table of counts
       

```
table(workshop)
       table(gender, workshop)
       mycounts <- table(gender, workshop)
       mode(mycounts)
       class(mycounts)
```


     \# counts in summary/aggregate stype
    `  mycountsDF <- as.data.frame(myCounts)`
      \# clean up
     

```
 mydata["Zq1"] <- NULL
      rm(myAgg1, myAGG2)
```



    \# the plyr and reshape2 packages
  \#PYTHON
  \#SAS
    \#get means of q1 for each gender
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
  \# merge aggregated data back into mydata;
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
  
14. By or Split-file processing

  SAS:
  


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



R:



```
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
```

   \# a data frame the long way
    

```
 myBYdata <- data.frame(
       rbind(myBYout[[1]], myBYout[[2]], 
             myBYout[[3]], myBYout[[4]])
     )
```


     
   \# a data frame using do.call
   
 `    myBYdata <- data.frame(do. call(rbind, myBYout))`
     
       
  
15. Removing duplicate obserations

SAS:


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


  
16. Selecting first or last observations per group

SAS:



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



R: 



```
mydata$id <- row.names(mydata)
mybys <- data.frame(mydata$workshop, mydata$gender)
mylastlist <- by(mydata, myBys, tail, n = 1)
```






\# back into a data frame


```
mylastDF <- do.call(rbind, mylastlist)
```



\# another way to create the data frame)


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


                  
17. Transposing or flipping data sets

SAS:


```
proc transpose data = mylib.mydata out = mycopy;
run;

proc transpose data = mycopy out = myFixed;
run;
```
R:



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
myFixed[ , myQs] <- lapply(myFixed[ , myQs], as.numeric)
```



