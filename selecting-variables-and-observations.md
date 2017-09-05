# Selecting variables and observations

SAS:



```
data mylib.mymalewq; s
et mylib.mydata; 
where gender = "m"; 
keep workshop q1 -q4; 
run;
```

R: 

\\# 1. the subset function



```
myMalesWQ <- subset(mydata, subset = gender == "m", select = c(workshop, q1 :q4))
```



\\# 2. logic for obs, names for vars



```
myMales <- which(gender == "m") 
myVars <- c("workshop", paste(q, 1:4, sep = "")) myMalesWQ <- mydata[myMales, myVars]
```



\\#3. row and variable names



```
myMales <- c("5", "6", "7", "8") 
myVars <- c("workshop", "q1", "q2", "q3", "q4")
```



\\#4. numeric index vectors



```
myMales <- c(5,6,7,8) 
myVars <- c(1,3,6) 
print(mydata[myMales, myVars)
```



\\#5. saving and loading subsets



```
myMalesWQ <- subset(mydata, subset = gender == "m", select = c(workshop, q1 :q4) 
save\(mydata, myMalesWQ, 
file = "myboth.RData") 
load("myBoth.RData') 
save(myMalesWQ, file = "myMalesWQ.RData") 
load("myMalesWQ.RData)
```





















