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

### 1. use the series's index

`in: obj = series(np.arrange(4.), index = ['a', 'b', 'c', 'd'])`

`obj['b']`

or

`obj[1]`

or

`obj[2:4]`

or

`obj[[ 'b', 'a', 'd']]`

or

`obj[[1, 3]]`

or

`obj[obj <2]`

or

`obj['b':'c'] = 5`

## SAS:





