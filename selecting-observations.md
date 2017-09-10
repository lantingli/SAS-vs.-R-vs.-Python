## R:

### 1. Selecting observations by indexs

#### a. print all rows

```
   print(mydata[ ]) ; 
   print(mydata[, ]); 
   print(mydata[1:8])
```

#### b. Just observation 5:

```
print(mydata[5, ])
```

#### c. multiple

```
print(mydata[ c(5,6,7,8), ])
```

#### d. use indice

```
   myMindices <- c(5,6,7,8) ; 
   summary(mydata[myMindices, ])
```

#### e. print a list of index numbers for each observation

```
data.frame(myindex = 1:8, mydata)
```

#### f. Select data using length as the end

```
print(mydata[1:nrow(mydata), ])
```

### 2. By row names

#### a. Display row names

  
`row.names (mydata)`

`print(mydata[ c("1", "2", "3", "4"), ])`

#### b. Assign new names

```
mynames <- c("Ann", "Cary", "Sue") ; 
row.names(mydata) <- mynames 
print(mydata["Ann", ]) 
print(mydata [c("Ann", "Cary", "Sue", "Carla"), ]) 
myNames <- c("Ann", "Cary", "Sue"); 
print(mydata[mynames, ])
```

### 3. By logic

#### a. 

`myRows <- c(TRUE, TRUE, TRUE, TRUE, FALSE, FALSE, FALSE, FALSE) ; `

`print(mydata[myRows,])`

#### b. select first four rows using 1 and 0: 

`myBinary <- c(1,1,1,1,0,0,0,0) ;`

`print(mydata[myBinary, ])`

`myRows <- as.logical(myBinary) ; `

`print(mydata\[myRows, ])`

#### c. 

`print(mydata[mydata$gender == "f", ]) `

`print(mydata[which(mydata$gender == "f"), ])`

#### d. using a saved vector 

`myFemales <- which(mydata$gender == "f” ） `

`print(mydata[myFemales, ])`

#### e. using %in% 

`myRsas <- which(mydata$workshop %in% c("R", "SAS")); `

`print(myRsas) ; `

`print(mydata[myRsas, ])`

#### f. equvalent selections using different ways to refer to the variables

```
 print(subset(mydata, gender == 'f') )
  attach(mydata) 
  print(mydata[which(gender == 'f' ), ]) detach(mydata) 
  with(mydata, print(mydata[which(gender == 'f'), ])) 
  print(mydata[which(mydata['gender'] == "f"), ]) 
  print(mydata[which(mydata$gender =="f"), ])
```

### 4. By string search

search for row names that begin with “C”

```
myCindices <- grep("^C", row.names(mydata), value = FALSE) 

print(mydata[myCindices , ])
```

###  5. By subset function

```
subset(mydata, subset = gender == "f") 
summary( subset(mydata, subset = gender = "m" & q4 ==5) ）
```

### 6. Generating indices A to Z from two row names

```
myMaleA <- which(row.names(mydata) == "Bob") 
myMaleZ <- which(row.names(mydata) == "Rich") print(mydata[myMaleA :myMaleZ, ])
```

### 7. Creating a new data frame of selected observations

```
a. myMales <- mydata [5:8, ] 
b. myMales <- mydata[which(mydata$gender =="m"), ] 
c. myMales <- subset\(mydata, subset = gender == "m")
```



