## 1.Tranforming variables

### R：

```
    setwd("c:/myRfolder") 
    load(file = "mydata.RData")
```

#### a. Transformation in the middle of another function

```
summary(log(mydata$q4)
```

#### b. Creating meanQ with dollar notation

```
mydata$meanQ <- (mydata$q1 +mydata$q2+mydata$q3+mydata$q4) /4
```

#### c. Creating two variables using transfrom

```
mydata <-- transform(mydata, score1 = (q1+q2)/2, score2 = (q3+q4)/2)
```

#### d. Creating meanQ using index notation on the left

```
load(file = "mydata.RData")
mydata <- data.frame(cbind(mydata, mean   q =0.))
mydata[7] <- (mydata$q1 +mydata$q2 +mydata$q3    +mydata$q4)/4
```

### PYTHON:

### SAS:

```
    LIBNAME mylib 'C:\myRfolder';
      data mylib.mydataTransformed;
        set mylib.mydata;
        totalq = (q1+q2+q3+q4);
        logtot = log10(totalq);
        mean1 = (q1+q2+q3+q4)/4;
        mean2 = mean(of q1 - q4)
```

## 2. Procedures or functions

### R

#### a. Mean of the q variables

     `mean(mydata[3:6], na.rm = TURE)`

#### b. Create mymatrix

     `mymatrix <- as.matrix(mydata[ , 3:6])`

#### c. Get mean of whole matrix

     `mean(mymatrix, na.rm = TRUE)`

#### d. get mean of matrix columns.

     `apply(mymatrix, 2,mean, na.rm= TRUE)`

#### e. get mean of matrix rows

```
     apply(mymatrix, 1, mean, na.rm = TRUE)
     rowMeans (mymatrix, na.rm = TRUE)
```

#### f. add row means to mydata

```
     mydata$meanQ <- apply(mymatrix, 1, mean, na.rm = TRUE) 
     mydata$meanQ <- rowMeans (mymatrix, na.rm = TRUE)
     mydata <- transform(mydata, meanQ =  rowMeans(mymatrix, na.rm = TRUE))
```

#### g. Means of data frames & their vectors

```
     lapply(mydata [, 3:6], mean, na.rm = TRUE)
     sapply(mydata [, 3:6], mean, na.rm = TRUE)
     mean(
     sapply(mydata [, 3:6], mean, na.rm = TURE)
     )
```

#### h. Length of data frames & their vectors

```
    length(mydata[, "q3"])
    nrow(mydata)
    is.na(mydata[, "a3"])
    !is.na(mydata [ , "q3"])
    sum(!is.na(mydata [, "q3"]))
```

### PYTHON:

### SAS:

`data mylib.mydata;`

`set mylib.mydata;`

`myMean = MEAN(OF q1-q4);`

`myN = N(OF q1- q4);`

`run;`

`proc means ;`

`var q1 - q4 myMean myN;`

`run;`

## 3. Finding N or NVALID

### R:

`library("prettyR")`

`sapply(mydata, valid.n)`

`apply(myMatrix, 1, valid.n)`

`mydata$myQn <- apply(myMatrix,1, valid.n)`

## 4. Standardizing and ranking variables

### R:

`myZs <- apply(mymatrix, 2,scale)`

`myRanks <- apply(mymatrix, 2, rank)`

### PYTHON:

### SAS:

```
     proc standard data = mylib.mydata;
       mean = 0 std = 1 out = myzs;
     run;

     proc rank data = mylib.mydata out = myranks;
     run;
```

## 5. applying your own functions

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

## 6. Conditional transformations

### R

`setwd("c:/myRfolder")`

`load(file = "mydata.RData")`

`mydata$q4Sagree <- ifelse(q4 ==5, 1,0)`

`mydata$q4agree <- as.numeric(q4 ==5)`

`mydata$q4agree <- ifelse(q4 >= 4, 1,0)`

`mydata$ws1agree <- ifelse(workshop = 1& q4 >= 4, 1,0)`

`mydata$score <- ifelse(gender =="f", (2*q1) +q2, (3*q1) +q2)`

### PYTHON:

### SAS:

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

## 7. cutting functions

### R:

`attach(mydata100)`

#### a. an inefficient approach

```
     postgroup <- posttest 
     postgroup <- ifelse(posttest <60           , 1, postgroup)
     postgroup <- ifelse(posttest >= 60 & posttest <70, 2, postgroup)
     postgroup <- ifelse(posttest >= 70 & posttest <80, 3, postgroup)
     postgroup <- ifelse(posttest >= 80 & posttest <90, 4, postgroup)
     postgroup <- ifelse(posttest >= 90,                5, postgroup)
```

#### b. an efficient approach

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

#### c. Logical approach

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

### PYTHON:

### SAS:

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

## 8. Multiple conditional transformation

### R:

#### a. Using the ifelse approach

`mydata$score1 <- ifelse(gender == "f", (2q1) +q2, #score1 for females;    
  (20q1+q2)  # score1 for males    
  )`

`mydata$score2 <- ifelse(gender =="f",     
  (3q1+q2), # score2 for females    
   (30q1 +q2) # score 2 for males    
   )`

#### b. Using the index approach

l`oad(file = "mydata.Rdata")`

`create names in data frame`

`mydata <- data.frame(mydata, score1 = NA, score2 = NA)`

`attach(mydata)`

`\# find which are males and females`

`gals <- which(gender == "f")`

`guys <- which(gender =="m")`

`mydata[gals, "socre1"] <- 2*q1[gals] +q2[gals]`

`mydata[gals, "score2"] <- 3 *q1[gals] +q2[gals]`

`mydata[guys, "score1"] <- 20* q1[guys] + q2 *[guys]`

`mydata[guys, "score2"] <- 30 * q1[guys] +q2[guys]`

`\# clean up`

`rm(guys, gals)`

### PYTHON：

###  SAS：

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
    run；
```

## 9. Missing values

When importing numeric data, R reads blanks as missing\(except when blanks are delimiters\). R reads the string NA as missing for both numeric and character variables. when importing a text file, both SAS and SPSS would recognize a period as a missing value for numeric variables. R will instead read the whole variable as a character vector!

###   SAS

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

### PYTHON:

### R:

`mydataNA <- read.table("mydataNA.txt")`

\\#read it so that ".", 9, 99 are missing.

`mydataNA <- read.table("mydtaNA.txt",  na.strings = c(".", "9", "99"))`

\\# convert 9 and 99 manually

`mydataNA <- read.table("mydataNA.txt", na.string = ".")`

`mydataNA [mydataNA ==9 | mydataNA ==99] <-NA`

\\# substitute the mean for missing values

 `mydataNA$q1 [is.na(mydataNA$q1)] <- mean(mydataNA$q1, na.rm = TRUE)`

\\#eliminate observations with any NAs

`myNoMissing <- na.omit(mydataNA)`

\\# finding complete observations

`complete.cases(mydataNA)`

\\# use that result to select complete cases

`myNoMissing <- mydataN[complete.cases(mydataNA), ]`

\\# use that result to select incomplete cases

`myincomplete <- mydataNA [!complete.cases(mydataNA), ]`

\\# when "99" has meaning 

`mydataNA <- read.table("mydataNA.txt", na.strings = ".")`

\\# assign missing values for q variables

`mydataNA$q1 [q1 == 9] <- NA`

`mydataNA$q2[q2 ==9] <- NA`

`mydataNA$q3 [q3 ==99] <- NA`

`mydataNA$q4 [q4 == 99] <- NA`

## 10. Use our function

### R:

```
       my9isNA <- function(X){x[x==9] <- NA; x}
       my99isNA <- function(x) {x[x==99]<- NA; x}

       mydataNA[3:4] <- lapply(mydataNA[3:4, my9isNA)
       mydataNA[5:6] <- lapply(mydataNA[5:6], my99isNA)
```

### PYTHON

pandas uses floating value NaN to represent missing data in both floating as well as in non-floating point arrays.

In: string\_data = series\(\['aardvark', 'articoke', np.nan, 'avocado'\]\)

string\_data.isnull\(\)

## 11. Renaming variables

### R:

####    a. using the data editor

  
   ` fix(mydata)  
    Restore original names for next example  
    names(mydata) <- c("workshop", "gender", "q1", "q2", "q3", "q4")`

####    b. using the reshape2 pakage

  
     `library("reshape2")  
    myChanges <- c(q1 = "x1", q2 = "x2", q3 = "x3", q4 = "x4")  
    mydata <- rename(mydata, myChanges)`

####     c.  the standard R approach

  
     `names(mydata) <- c("workshop", "gender", "x1", "x2", "x3", "x4")`

####     d. Using the edit function

  
    ` names (mydata) <- edit(names(mydata))`

### PYTHON:

###  SAS:

## 12. Renaming by index

### R: 

```
   mynames <- names(mydata)
   data.frame(mynames))
   mynames[3] <- "x1"
   mynames[4] <- "x2"
   mynames[5] <- "x3"
   mynames[6] <- "x4"
   names(mydata) <- mynames
```

a. renaming by column name

```
 mynames <- names(mydata)
 mynames[mynames == "q1"] <- "x1"
 mynames[mynames =="q2"] <- x2"
 mynames [mynames == "q3"]<- "x3"
 mynames[mynames == "q4"] <- "x4"
 names(mydata) <- mynames
```

b. renaming many sequentially numbered variable

`names(mydata)  
 myXs <- paste("x", 1:4, sep = "")  
 myA <- which(names(mydata) == "q1")  
 myZ <- which(names(mydata) =="q4")`

`names(mydata) [myA:myZ] < myXs(mydata)`

### PYTHON:

### SAS:

```
     data mylib.mydata;
     rename q1 - q4 = x1-x4;
     run;
     
```

## 13. Recording variables



a.  recoding many variables

### R:

library\("car"\)

mydata$qr1 &lt;- recode\(q1, "1=2; 5= 4"\)

mydata$qr2 &lt;- recode\(q2, "1=2; 5= 4"

mydata$qr3 &lt;- recode\(q3, "1=2; 5=4"\)

mydata$qr4 &lt;- recode\(q4, 1=2; 5=4"\)

### PYTHON:

### SAS: 

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

## 14. Indicator or Dummy variables

###  R:

`load("mydata100.RData")  
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
head(class.ind(workshop))`

### PYTHON:

###  SAS:

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

## 15. Keeping and Dropping variables

### R:

#### a. using variable selection

```
myleft <- mydata[, 1:4]
```

#### b. using NULL

```
myleft <- mydata
myleft$q3 <-mydata$q4 <- NULL
```

### PYTHON:

#### a. reindexing:

```py
in: obj = series [(4.5, 7.2, -5.3, 3.6], index = ['d', 'b', 'a', 'c'])
```

calling reindex on this series rearranges the data according to the new index, introducing missing if any index values were not already present:

```
in: obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])

out :

a -5.3
b 7.2
c 3.6
d 4.5
e NaN

or obj.reindex(['a', 'b', 'c', 'd', 'e'], fill_value = 0)

or 
in: obj3 = series(['blue', 'purple', 'yellow'], index = [0,2,4])
obj3.reindex (range(6), method = 'ffill')
```

reindex method \(interpolation\) options

ffill or pad: fill \(or carry\) values forward  
bfill or backfill: fill\(or carry\) values backford

With dataframe, reindex can alter either the \(row\) index, columns, or both. when passed just a sequence, the rows are reindexed in the result:

#### b. dropping entries from an axis

dropping one or more entries from an axis is easy if you have an index array or list without those entries. as that can require a bit of munging and set logic, the drop method will return a new object with the indicated value or values deleted from an axis:

```
in: obj= series(np.arrange(5.), index = ['a', 'b', 'c', 'd', 'e'])
new_obj = obj.drop ('c')
```

with dataframe, index values can be deleted from either axis:

```
in : data = dataframe(np.arrange(16).reshape((4,4)),
 index = ['ohio', 'colorado', 'utah', 'new york'], columns = ['one' , 'two', 'three', 'four'])

 data.drop (['Colorado', 'ohio']
 data.drop('two', axis = 1)
 data.drop('two', 'four'], axis = 1)
```

###  SAS:

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



