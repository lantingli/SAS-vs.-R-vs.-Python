18. Reshaping variables to observations and back

SAS:




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
R:



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


     
     

19. sorting data frames

SAS :
proc sort data = mylib.mydata;
  by workshop;
run;

proc sort data = mylib.mydata;
  by descending gender workshop;
  run;
  
  R:
  


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


    
    


20. Converting data structure
21. Character string manipulations
22. Dates and Times

\# Calculating durations

\# adding durations to date-time variables

\# accessing date-time elements

\# creating date-time variables from elements

\# logical comparisons with date-time variables

\# formatting date-time output

\# two-digit years

\# date-time conclusion
23. Loops
24. Functions1
8. Reshaping variables to observations and back

SAS:



```
proc transpose data = mylib.mydata
out = mylib.mylong;
var q1 - q4;
by id workshop gender;
run;
```


19. sorting data frames
20. Converting data structure
21. Character string manipulations
22. Dates and Times

\# Calculating durations

\# adding durations to date-time variables

\# accessing date-time elements

\# creating date-time variables from elements

\# logical comparisons with date-time variables

\# formatting date-time output

\# two-digit years

\# date-time conclusion
23. Loops
24. Functions