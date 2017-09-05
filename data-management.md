iiiiiiiiiiiiii

1. Tranforming variables

a. R：

```
    setwd("c:/myRfolder") 
    load(file = "mydata.RData")
```
   \# Transformation in the middle of another function
```
     summary(log(mydata$q4)
```
   \# Creating meanQ with dollar notation

    mydata$meanQ <- (mydata$q1 +mydata$q2+mydata$q3+mydata$q4) /4

   \# Creating two variables using transfrom

    mydata <-- transform(mydata, score1 = (q1+q2)/2, score2 = (q3+q4)/2)

   \# Creating meanQ using index notation on the left 

```
    load(file = "mydata.RData")
      mydata <- data.frame(cbind(mydata, mean   q =0.))
      mydata[7] <- (mydata$q1 +mydata$q2 +mydata$q3    +mydata$q4)/4
```

b. PYTHON:

c. SAS:
```
    LIBNAME mylib 'C:\myRfolder';
      data mylib.mydataTransformed;
        set mylib.mydata;
        totalq = (q1+q2+q3+q4);
        logtot = log10(totalq);
        mean1 = (q1+q2+q3+q4)/4;
        mean2 = mean(of q1 - q4);
```



2. Procedures or functions


  \#1)Applying the mean function
     
  \#a. R
        
 \# Mean of the q variables
     `mean(mydata[3:6], na.rm = TURE)`
  \# Create mymatrix
     `mymatrix <- as.matrix(mydata[ , 3:6])`
  \#  Get mean of whole matrix
     `mean(mymatrix, na.rm = TRUE)`
  \# get mean of matrix columns.
     `apply(mymatrix, 2,mean, na.rm= TRUE)`
  \# get mean of matrix rows
  
```
     apply(mymatrix, 1, mean, na.rm = TRUE)
     rowMeans (mymatrix, na.rm = TRUE)
```

  \# add row means to mydata
    

```
     mydata$meanQ <- apply(mymatrix, 1, mean, na.rm = TRUE) 
     mydata$meanQ <- rowMeans (mymatrix, na.rm = TRUE)
     mydata <- transform(mydata, meanQ =  rowMeans(mymatrix, na.rm = TRUE)
     )
```


  \# Means of data frames & their vectors
     

```
     lapply(mydata [, 3:6], mean, na.rm = TRUE)
     sapply(mydata [, 3:6], mean, na.rm = TRUE)
     mean(
     sapply(mydata [, 3:6], mean, na.rm = TURE)
     )
```

 \# Length of data frames & their vectors
 

```
    length(mydata[, "q3"])
    nrow(mydata)
    is.na(mydata[, "a3"])
    !is.na(mydata [ , "q3"])
    sum(!is.na(mydata [, "q3"]))
```
 
   python:
   SAS:
```
    data mylib.mydata;
     set mylib.mydata;
       myMean = MEAN(OF q1-q4);
       myN = N(OF q1- q4);
    run;
    
    proc means ;
       var q1 - q4 myMean myN;
    run;
```



     \#Finding N or NVALID
     R: 
```
     library("prettyR")
     sapply(mydata, valid.n)
     apply(myMatrix, 1, valid.n)
     mydata$myQn <- apply(myMatrix,1, valid.n)
```



     \#standardizing and ranking variables
     
     R:


```
     myZs <- apply(mymatrix, 2,scale)
     myRanks <- apply(mymatrix, 2, rank)
```


     
     PYTHON:
     SAS:
```
     proc standard data = mylib.mydata;
       mean = 0 std = 1 out = myzs;
     run;
   
     proc rank data = mylib.mydata out = myranks;
     run;
```



 \# applying your own functions

  `apply(mymatrix, 2, mean, sd) # No good`
   
 

```
    mystats <function(x) {
     c(mean= mean(x, na.rm = TRUE),
       sd = sd(x, na.rm = TRUE))
       }
    apply(mymatrix, 2, mystats)
    apply(mymatrix, 2, function(x) {
        c(mean = mean(x, na.rm = TRUE), 
         sd = sd(x, na.rm = TRUE))
         })
```


3. Conditional transformations

     \# the ifelse function
     
      \#a. R
      

```
      setwd("c:/myRfolder")
      load(file = "mydata.RData")
      mydata$q4Sagree <- ifelse(q4 ==5, 1,0)
      mydata$q4agree <- as.numeric(q4 ==5)
      mydata$q4agree <- ifelse(q4 >= 4, 1,0)
      mydata$ws1agree <- ifelse(workshop = 1& q4 >= 4, 1,0)
      mydata$score <- ifelse(gender =="f", (2*q1) +q2, (3*q1) +q2)
```


      \#b. python
      \#c. SAS
      


```
      LIBNAME mylib 'C:\myRfolder';
      data mylib.mydataTransformed;
        set mylib.mydata;
        if q4 = 5 then x1 =1 ; else x1 = 0;
        if q4 >= 4 then x2 =1; else x2 = 0;
        if workshop = 1 & q4 >= 5 then x3 = 1;
          else x3 = 0;
          if gender = "f" then scoreA = 2*q1 +q2;
                               else scoreA  = 3* q1 +q2;
          if workshop = 1 and q4 >= 5 
            then scoreB = 2*q1 +a2;
            else scoreB = 3*q1 +q2;
         run;
```



  \# 4.  cutting functions
  \#a. R
  
  `attach(mydata100)`
  
\#an inefficient approach
 ```    
     postgroup <- posttest 
     postgroup <- ifelse(posttest <60           , 1, postgroup)
     postgroup <- ifelse(posttest >= 60 & posttest <70, 2, postgroup)
     postgroup <- ifelse(posttest >= 70 & posttest <80, 3, postgroup)
     postgroup <- ifelse(posttest >= 80 & posttest <90, 4, postgroup)
     postgroup <- ifelse(posttest >= 90,                5, postgroup)
```

\#an efficient approach
```
    postgroup <- 
     ifelse(posttest <60                 , 1,
       ifelse(posttest>= 60 & posttest <70, 2,
          ifelse(posttest >= 70 & posttest <80, 3, 
          ifelse(posttest >= 80 & posttest <90, 4,
             ifelse (posttest >= 90            , 5, posttest)
     ))))
    table(postgroup)
```
\# Logical approach
  

```
   postgroup <- 1 +
      (posttest >= 60) +
      (posttest >= 70) +
      (posttest >= 80) +
      (posttest >= 90)
      table(postgroup)
```



```
library("Hmisc"）
postgroup <- cut2(posttest, c(60,70, 80, 90))
table(postgroup)
postgroup<- cut2(posttest, g = 5)
postgroup <- cut2(postest, m =25)
```




  \#b. python
  \#c. SAS
  
 

```
    data mylib.mydataTransformed;
    set mylib.mydata100;
    if (posttest <60) then postGroup = 1;
    else if(posttest >= 60 & posttest <70) then postgroup = 2;
    else if(posttest >= 70 & posttest <80) then postgroup = 3;
    else if (posttest >= 80 & posttest <90) then postgroup = 4;
    else if (posttest >= 90) then postegroup = 5;
    run;
 
    proc freq;
      tables postgroup;
    run;
 
    proc rank out = mylib.mydataTransformed Groups = 5;
      var posttest;
    run;
```


     

5. Multiple conditional transformation

\#R

\# using the ifelse approach

  mydata$score1 <- ifelse(gender == "f", (2*q1) +q2, #score1 for females;
  (20*q1+q2)  # score1 for males
  )
  
  mydata$score2 <- ifelse(gender =="f", 
  (3*q1+q2), # score2 for females
   (30 *q1 +q2) # score 2 for males
   )
   
\# using the index approach
  load(file = "mydata.Rdata")
      \#create names in data frame


```
      mydata <- data.frame(mydata, score1 = NA, score2 = NA)
       attach(mydata)
       \# find which are males and females
       gals <- which(gender == "f")
       guys <- which(gender =="m")
       mydata[gals, "socre1"] <- 2*q1[gals] +q2[gals]
       mydata[gals, "score2"] <- 3 *q1[gals] +q2[gals]
       mydata[guys, "score1"] <- 20* q1[guys] + q2 *[guys]
       mydata[guys, "score2"] <- 30 * q1[guys] +q2[guys]
       
        \# clean up
        rm(guys, gals)
```


  
\#PYTHON
\# SAS



```
    data mylib.mydata;
    set mylib.mydata;
    if gender = "f" then do;
    score1 = (2*q1) +q2;
    score2 = (3*q1) +q2;
    end;
    else if gender = "m" then do;
    score1 = (20*q1) +q2;
    score2 = (30*q1)+q2;
    end;
    run;
```





6. Missing values
\# when importing numeric data, R reads blanks as missing(except when blanks are delimiters). R reads the string NA as missing for both numeric and character variables. when importing a text file, both SAS and SPSS would recognize a period as a missing value for numeric variables. R will instead read the whole variable as a character vector!
\# SAS 


```
    data mylib.mydata;
     set mylib.mydata;
       if q1= 9 then q1 = .;
       if q2 = 9 then q2 = .;
       if q3 = 99 then q3 = .;
       if q4 = 99 then q4 = .;
    \# same thing but is quicker for lots of vars
       array q9 q1 -q2;
         do over q9;
           if q9 = 9 then q = .;
        end;
        array q99 q3 - q4;
          do over q99;
            if q = 99 then q99 = .;
        end;
```

\# R


```
mydataNA <- read.table("mydataNA.txt")
  \#read it so that ".", 9, 99 are missing.
  mydataNA <- read.table("mydtaNA.txt", 
    na.strings = c(".", "9", "99"))
    
    \# convert 9 and 99 manually
    mydataNA <- read.table("mydataNA.txt",
    na.string = ".")
    mydataNA [mydataNA ==9 | mydataNA ==99] <-NA
    \# substitute the mean for missing values
     mydataNA$q1 [is.na(mydataNA$q1)] <- mean(mydataNA$q1, na.rm = TRUE)

```


    \eliminate observations with any NAs
    `myNoMissing <- na.omit(mydataNA)`
    

     \# finding complete observations
     
     `complete.cases(mydataNA)`
     \# use that result to select complete cases
      `myNoMissing <- mydataN[complete.cases(mydataNA), ]`
     \# use that result to select incomplete cases
     `myincomplete <- mydataNA [!complete.cases(mydataNA), ]`
     

     \# when "99" has meaning 
     `mydataNA <- read.table("mydataNA.txt", na.strings = ".")`
     
     \# assign missing values for q variables
     

```
      mydataNA$q1 [q1 == 9] <- NA
      mydataNA$q2[q2 ==9] <- NA
      mydataNA$q3 [q3 ==99] <- NA
      mydataNA$q4 [q4 == 99] <- NA
```


      
      \#use our funcion



```
       my9isNA <- function(X){x[x==9] <- NA; x}
       my99isNA <- function(x) {x[x==99]<- NA; x}
       
       mydataNA[3:4] <- lapply(mydataNA[3:4, my9isNA)
       mydataNA[5:6] <- lapply(mydataNA[5:6], my99isNA)
```



7. Renaming variables
 
    \# R

      \# advanced renaming 
   
     \# using the data editor
       fix(mydata)
     \# Restore original names for next example
        names(mydata) <- c("workshop", "gender", "q1", "q2", "q3", "q4")
      
      \# using the reshape2 pakage
        library("reshape2")
        myChanges <- c(q1 = "x1", q2 = "x2", q3 = "x3", q4 = "x4")
        mydata <- rename(mydata, myChanges)
      
      \# the standard R approach
        names(mydata) <- c("workshop", "gender", "x1", "x2", "x3", "x4")
        
      \#Using the edit function
        names (mydata) <- edit(names(mydata))
      
   \#python
   \#sas
   
 \# renaming by index
 


```
   mynames <- names(mydata)
   data.frame(mynames))
   mynames[3] <- "x1"
   mynames[4] <- "x2"
   mynames[5] <- "x3"
   mynames[6] <- "x4"
   names(mydata) <- mynames
```



\# renaming by column name
 

```
mynames <- names(mydata)
 mynames[mynames == "q1"] <- "x1"
 mynames[mynames =="q2"] <- x2"
 mynames [mynames == "q3"]<- "x3"
 mynames[mynames == "q4"] <- "x4"
 names(mydata) <- mynames
```


 

\# renaming many sequentially numbered variable

names(mydata)
myXs <- paste("x", 1:4, sep = "")
myA <- which(names(mydata) == "q1")
myZ <- which(names(mydata) =="q4")

names(mydata) [myA:myZ] < myXs(mydata)

\# SAS
```
     data mylib.mydata;
     rename q1 - q4 = x1-x4;
     run;
```



8. Recording variables

      \# recoding a few variables

      \# recoding many variables

       \#R


```
       library("car")
       mydata$qr1 <- recode(q1, "1=2; 5= 4")
       mydata$qr2 <- recode(q2, "1=2; 5= 4")
       mydata$qr3 <- recode(q3, "1=2; 5=4")
       mydata$qr4 <- recode(q4, 1=2; 5=4")
```


       
       \#not sure what this part is about!!!
      
      \#SAS
      
    

```
  LIBNAME mylib 'C:\myRfolder';
      data mylib.mydata;
        infile '$\myRfolder\mydata.csv' csv$ delimiter = '$,', $ MISSOVER DSD LRECL = 32767 firstobs = 2;
        INPUT id workshop gender $ q1 q2  q3 q4;
        proc print ; run;
        proc format;
        value agreement 1= "Disagree" 2= "Disagree"
          3 = "Neutral" 4 = "Agree"; 
        run;
        
      data mylib.mydata;
        set mylib.mydata;
          array q q1 - q4;
          array qr qr1 - qr4;
          do i = 1 to 4;
          qr{i} = q{i}
            if q{i} =  then qr{i} = 2;
            else 
            if q{i} = 5then qr{i} = 4;
          end;
          format q1 - q4 agreement;
        run;
        
        \#this will use the recorded formats automatically
        proc freq;
          tables q1 - q4;
        run;
        \# this will ignore the formats
        proc univariate;
          var q1-q4;
        run;
        proc univariate;
          var qr1 - qr4;
        run;
            

```


9. indicator or Dummy variables
\#R

 load("mydata100.RData")
 attach(mydata100)
 r <- as.numeric(workshop == "R")
 sas <- as.numeric(workshop == "SAS")
 spss <- as.numeric(workshop == "spss")
 stata <- as.numeric(workshop == "Stata")
 head(data.frame(workshop, r, sas, spss, stata))
 lm(posttest ~ pretest +sas+spss+stata)
 lm(posttest ~ prestest +workshop)
 workshop <- relevel (workshop, "SAS")
 coef(lm(posttest ~ pretest +workshop))
 library("nmet")
 head(class.ind(workshop))
 
\#python
\#SAS 


```
 data temp;
  set mylib.mydata100;
    r = workshop = 1;
    sas = workshop = 2;
    spss = workshop = 3;
    stat = workshop = 4;
  run;
  
  proc reg;
    model posttest = pretest sas spss stata;
  run;
```


    

10. Keeping and Dropping variables

\# R

\#using variable selection



```
myleft <- mydata[, 1:4]
```



\#using NULL


```
myleft <- mydata
myleft$q3 <-mydata$q4 <- NULL
```



\# PYTHON
\# SAS



```
data myleft;
 set mydata;
 keep id workshop gender q1 q2;
run;

data myleft;
  set mydata;
    drop q3 q4;
run;

```

  
 
   
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
\# sas



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


  
3. Selecting first or last observations per group

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


                  
4. Transposing or flipping data sets
5. Reshaping variables to observations and back
6. sorting data frames
7. Converting data structure
8. Character string manipulations
9. Dates and Times

   \# Calculating durations

   \# adding durations to date-time variables

   \# accessing date-time elements

   \# creating date-time variables from elements

   \# logical comparisons with date-time variables

   \# formatting date-time output

   \# two-digit years

   \# date-time conclusion





















Transforming variables:

R

`mydata$meanQ <- (mydata$q1 +mydata$q2) /2`

Python

SAS

`data a;`

`set b;`

`keep var1 var2 var3;`

`run;`

| Transforming variables |  |  |
| :--- | :--- | :--- |
|  | R |  |
|  | Python |  |
|  | SAS |  |

|  | R | PYTHON | SAS |
| :--- | :--- | :--- | :--- |
| **1. Transforming variables** |  |  |  |
| **2. Procedures or Functions** |  |  |  |
| _ Applying the mean function_ |  |  |  |
| _Finding N or NVALID_ |  |  |  |
| _Standardizing and ranking variables_ |  |  |  |
| _ Applying your own functions_ |  |  |  |
| **3. Conditional Transformations** |  |  |  |
| _ifelse function_ |  |  |  |
| _Cutting functions_ |  |  |  |
| **4. Multiple conditional transformations** |  |  |  |
| **5. Missing values** |  |  |  |
| _Substituting means for missing values_ |  |  |  |
| _Finding complete observations_ |  |  |  |
| _when "99" has meaning_ |  |  |  |
| **6. Renaming variables\(and observations\)** |  |  |  |
| _Renaming by index_ |  |  |  |
| _Renaming by column names_ |  |  |  |
| _Renaming many sequentially numbered variable_ |  |  |  |
| _Renaming observations_ |  |  |  |
| **7. Recoding variables** |  |  |  |
| _Recoding a few variables_ |  |  |  |
| _Recoding many variables_ |  |  |  |
| **8. Indicator or dummy variables** |  |  |  |
| **9. Keeping and dropping variables** |  |  |  |
| **10. Stacking/Concatenating/Adding datasets** |  |  |  |
| **11. Joining/Merging data sets** |  |  |  |
| **12. Creating summarized or aggregated data sets** |  |  |  |
| _The aggregate function_ |  |  |  |
| _The tapply function_ |  |  |  |
| _Merging aggregates with original data_ |  |  |  |
| _Tabular aggregation_ |  |  |  |
| **13. By or split -file processing** |  |  |  |
| **14. Removing duplicate observations** |  |  |  |
| **15. Selecting first or last observations per group** |  |  |  |
| **16. Transposing or flipping data sets** |  |  |  |
| **17. Reshaping or flipping data sets** |  |  |  |
| **18. Sorting data frames** |  |  |  |
| **19. Converting data structure** |  |  |  |
| **20. Character string manipulations** |  |  |  |
| **21. Dates nad times** |  |  |  |
| _Calculating durations_ |  |  |  |
| _Adding durations ot date-time variables_ |  |  |  |
| _Accessing date-time elements_ |  |  |  |
| _Creating date-time variables from elements_ |  |  |  |
| _Logical comparisons with date-time variables_ |  |  |  |
| _Formatting date-time output_ |  |  |  |

1. 
2. Transforming variables: calculate mean

3. Mydata$meanQ &lt;- \(mydata$q1 +mydata $q2

   * mydata$q3+mydata$q4\)\/4

4. Mydata\[, “meanQ”\]&lt;- \(mydata \[,”q1”\] + mydata \[,”q2”\]

5. mydata \[,”q3”\] + mydata \[, “q4”\]\)\/4

6. Within\(mydata,

   meanQ &lt;- \(q1 +q2+q3+q4\)\/4

   \)

7. Mydata &lt;- transform\(mydata, meanQ = \(q1+q2+q3+q4\)\/4\)

8. Attach\(mydata\)

Mydata$meanQ &lt;- \(q1 + q2+q3+q4\)\/4

Detach\(mydata\)

1. Using plyr package

Mydata &lt;- data.frame\(cbind\(mydata, meanQ = 0.\)\)

Mydata \(7\) &lt;- \(mydata$q1 + mydata$q2

+mydata$q3+mydata$q4\)\/4

1. Procedures or functions

2. Calculate mean group by some variable

3. Mean\(mydata\[3:6\]. Na.rm = TURE\)

4. Mymatrix &lt;- mydata \[, 3:6\]

Mymatrix &lt;- as.matrix\(mydata\[3:6\]\)

Mydata$meanQ &lt;- Apply\(mymatrix, 2, mean, na,rm = TRUE\)

1: representing rows, 2 representing columns

1. Mydata$meanQ &lt;-rowMeans\(mymatrix, na.rm = TURE\)
2. Mydata&lt;- transform \(mydata, meanQ = rowMeans \(mymatrix, na.rm = TRUE\)
3. Lapply\(mydata\[ , 3:6\], mean, na.rm = TURE\)
4. Sapply\(mydata\[ , 3:6\], mean, na.rm = TRUE\)

5. Finding N or NVALID

6. Length\(mydata\[ , “q3”\]\): count the valid values of those variables for each observation

7. Nrow\(mydata\)

8. Sum\(!is.na\(mydata \[ , “q3”\]\)\)

9. Using prettyR package: sapply\(mydata, valid.n\)

10. Mymatrix &lt;- as.matrix\(mydata \[ , 3:6\]\)

Apply\(mymatrix, 1, valid.n\)

1. Mydata$myQn &lt;- apply\(mymatrix, 1, valid.n\)

2. Standardizing and ranking variables

myZs &lt;- apply\(mymatrix, 2, scale\) = \(sas: proc standard\)

myRanks &lt;- apply\(mymatrix, 2, rank\)

1. Applying your own functions

Apply\(mymatrix, 2, mean, sd\) \# not good.

Should write like this:

Mystats &lt;- function\(x\) {

C\(mean = mean\(x, na.rm = TRUE\),

Sd = sd \(x, na.rm = TRUE\)\)

}

Apply\(mymatrix, 2, mystats\)

Or：

Apply\(mymatrix, 2, function\(x\) {

C\(mean = mean \(x, na.rm = TRUE\),

Sd = sd \(x, na.rm = TRUE\)\)

} \)

1. Conditional transformation

2. The ifelse function

Mydata$q4agree&lt;- ifelse\(q4 ==5, 1,0\)

Mydata$q4agree&lt;- as.numeric\(q4 ==5\)

Mydata$ws1agree&lt;- ifelse\(workshop==1 & q4&gt;=4, 1, 0\)

Mydata$score &lt;- ifelse\(gender == “f”,

\(2\*q1\) +q2,

\(3\*q1\) +q2

\)

Postgroup &lt;-

Ifelse\(posttest&lt;60 ,1,

Ifelse\(posttest&gt;=60 & posttest &lt;70, 2,

Ifelse\(posttest&gt;=70 & posttest&lt;80, 3

Ifelse\(posttest &gt;= 80 & posttest &lt;90 , 4,

Ifelse\(posttest &gt;= 90 , 5, posttest\)

\)\)\)\)

1. Cutting functions

Use Hmisc package

Postgroup &lt;- cut2\(posttest, c\(60, 70, 80, 90\)\) \/ cut point

Postgroup &lt;- cut2\(posttest, g= 5\) \/ group number

Postgroup &lt;- cut2\(posttest, m = 25\)

Table \(postGroup\)

1. Multiple conditional transformations
2. Missing values

3. When importing numeric data, R reads blanks as missing\(except when blanks are delimiters\). R reads the string NA as missing for both numeric and character variables. When importing a text file, both SAS and SPSS would recognize a period as a missing value for numeric variables. R will, instead, read the whole variable as a character vector. If you have control over the source of the data, it is best not to write them out that way.

When other values represent missing, you will, of course, have to tell R about them. The read.table function provides an argument, na.strings, that allows you to provide a set of missing values.

mydataNA &lt;- read.table \(“mydataNA.txt”,

na.strings = c\(“.”, “9”, “99”\)\)

1. Substituting means for missing values

There are several methods for replacing missing values with estimates of what they would have been. These methods include simple mean substitution, regression and the gold standard – multiple imputation.

mydataNA$q1 \[is.na\(q1\)\]&lt;- mean\(q1, na.rm = TURE\)

1. Finding complete observations

2. Omit all observations that contain any missing values with na.omit function

Mynomissing &lt;- na.omit \(mydataNA\)

1. Get the cases that have no missing values\(the same result as the na.omit function\)

Mynomissing &lt;- mydataNA \[complete.cases \(mydataNA\),\]

1. When “99” has meaning

My9isNA &lt;- function\(x\) { x\[x==9\] &lt;- NA; x}

Or

My9isNA &lt;- function\(x\) { ifelse\(x ==9\), NA, x\) }

My99isNA &lt;- function\(x\){ifelse\(x==99\), NA, x\) }

mydataNA &lt;- read.table\(“mydataNA.txt”, na.strings = “.”\)

attach（mydataNA）

mydataNA\[3:4\] &lt;- lapply \(mydataNA\[3:4\], my9isNA\)

mydataNA\[5:6\] &lt;- lapply \(mydataNA\[5:6\], my99isNA\)

1. Renaming variables\(observations\)

2. Using reshape2 package

Mychanges &lt;- c\(q1 = “x1”, q2 = “x2”, q3 = “x3”, q4 = “x4”\)

Mydata &lt;- rename \(mydata, mychanges\)

1. Names\(mydata\) &lt;- c\(“workshop”, “gender”, “x1”, “x2”, “x3”, “x4”\)
2. Names\(mydata\) \[2\] &lt;- “sex”
3. Renaming by index

Mynames &lt;- names\(mydata\)

Data,frame\(mynames\)

Mynames\[3\] &lt;- “x1”

Mynames\[4\] &lt;- “x2”

Mynames\[5\]&lt;- “x3”

Mynames\[6\] &lt;-“x4”

1. Renaming by column name

Mynames &lt;- names\(mydata\)

Mynames \[mynames ==”q1”\] &lt;- “x1”

Mynames \[mynames ==”q2”\] &lt;- “x2”

Mynames \[mynames ==”q3”\] &lt;- “x3”

Mynames \[mynames ==”q4”\] &lt;- “x4”

Names\(mydata\) &lt;- mynames

1. Renaming many sequentially numbered variable names

myXs &lt;- paste \(“x”, 1:4, sep = “ “\)

myA &lt;- which\(names\(mydata\) ==”q1”\)

myZ&lt;-which\(names\(mydata\) ==”q4”\)

names\(mydata\) \[myA:myZ\] &lt;- myXs

1. Recoding variables

Recoding is just a simpler way of doing a set of related IF\/THEN conditional treansformation.

1. Using car package

Mydata$qr1 &lt;- recode\(q1, “1=2; 5=4”\)

Mydata$qr2 &lt;- recode\(q2, “1=2; 5=4”

Mydata$qr3 &lt;- recode\(q3, “1=2; 5=4”\)

Mydata$qr4 &lt;- recode\(q4, “1=2; 5=4”\)

1. Indicator or dummy variables

You can create indicator variables using the ifelse function or using the recode function, but the easiest approach is the same as in SAS and SPSS: using logic to generate a series of zeros and ones:

R&lt; - as.numeric \(workshop == “R”\)

Sas&lt;- as.numeric\(workshop ==”SAS”\)

Spss &lt;- as.numeric\(workshop = “SPSS”\)

Stata &lt;- as.numeric\(workshop ==”Stata”\)

Head\(data.frame\(

Workshop, r, sas, spss, stat\)\)

1. Keeping and dropping variables

Keep: myleft &lt;- mydata\[ , 1:4\]

Drop : mydata$varname &lt;- NULL

Myleft &lt;- myadta

Myleft$q3 &lt;- myleft$q4&lt;- NULL

NULL is only used to remove components from data frames and lists. You cannot use it to drop elements of a vector, nor can you use to remove a vector by itself from your workspace.

Rm\(mydata$q4\) this does not work, only remove whole objects, can not remove variables from within a data frame.

1. Stacking \/concatenating\/adding data sets

2. Rbind function: but rbind function needs both data frames to have the exact variable names.

3. Using plyr package

Both &lt;- rbind.fill\(females, males\) : binds whichever variables it finds that match and then fills in missing values for those that do not.

1. Joining \/merging data sets

Both &lt;- merge\(myleft, myright, by.x = “id”, by.y = “id”\)

Both &lt;- merge\(myleft, myright,

By = c\(“id”, “workshop”\)\)

Both &lt;- merge\(myleft, myright, by.x = c\(“id”, “workshop”\)

By.y = c\(“subject”, “shortcourse”\)）

By default, SAS and SPSS keep all records regardless of whether or not they match\(a full outer join\). For observations that do not have matches in the other file, the merge function will fill them in with missing values. R takes the opposite approach, keeping only those that have a record in both\(an inner join\). To get merge to keep all records, use the argument all = TURE. You can also use all.x = TRUE to keep all of the records in the first file regardless of whether or not they have matches in the second. The all.y = TRUE argument does the reverse.

While SAS and SPSS can merge any number of files at once, base R can only do two at a time. To do more, you can use the merge\_all function in the reshape package.

1. Creating summarized or aggregated data sets

2. Directly

Myagg1 &lt;- aggregate\(q1,

By = data.frame\(gender\),

Mean, na.rm = TRUE\)

Myagg2&lt;- aggregate\(q1,

By = data.frame\(workshop, gender\),

Mean, na.rm = TRUE\)

1. Using tapply function

The aggregate function has an important limitation: only use it with functions that return single values. The tapply function works very similarly to the aggregate function but without this limitation.

Myagg2&lt;- tapply\(q1,

Data.frame \(workshop, gender\),

Mean, na.rm = TRUE\)

1. Merging aggregates with original data
2. Tabular aggregation
3. The plyr and reshape2 packages

4. By or split-file processing

5. Removing duplicate observations

6. Completely duplicate observations

Mynoduplicates &lt;- unique \(myduplicates\) \/ unique function removed them but did not show them.

1. myDuplicates$DupRecs &lt;- duplicated \(myDuplicates\)

mynoduplicates &lt;- myduplicates \[!DupRecs, -7\]

1. duplicate keys

mykeys &lt;- c\(“workshop”, “gender”\)

mydata$dupkeys &lt;- duplicated \(mydata\[ , mykeys\]\)

1. Selecting first or last observations per group

myBys &lt;- data.frame\(mydata$workshop, mydata$gender\)

mylastlist &lt;- by\(mydata, myBys, tail, n =1\)

\#back into a data frame

mylastDF &lt;- do.call\(rbind, mylastList\)

another way to create the data frame:

mylastDF &lt;- rbind \(mylastList\[\[1\]\],

mylastList\[\[2\]\],

mylastList\[\[3\]\],

mylastList\[\[4\]\]\)

1. Transposing or flipping data sets

myQs &lt;- c\(“q1”, “q2”, “q3”, “q4”\)

myQdf &lt;- mydata \[ , myQs\]

myFlipped &lt;- t\(myQdf\)

myFixed &lt;- as.data.frame\(t\(myFlipped\)\)

1. Reshaping variables to observations and back
2. Sorting data frames

Mydata\[c\(4,3,2,1\), \]

myW &lt;- order \(mydata$workshop\)

mydata \[myW, \]

mydata\[rev\(myW\), \]

myDGW &lt;- order \(

-as.numeric \(mydata $gender\),

Mydata$workshop

\) \/ the default order is ascending, to reverse this, placet the minus sign before any variable. However, this only works with numeric variables, and we have been sorting by factors. We can use as.numeric function to extract the numeric values that are a behind – the –scenes oart of any factor.

1. Converting data structures

2. Character string manipulations

Stringr package

1. Dates and timesR



