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





18. Reshaping variables to observations and back

SAS:



```
proc transpose data = mylib.mydata
  out = mylib.mylong;
    var q1 - q4;
    by id workshop gender;
  run;
```


  
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






















