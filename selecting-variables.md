# 

**Selecting variables**

\\# by index number

  \\#1. Select all variables by default:

```
print(mydata[ ]); print (mydata[, ]);
```

  \\#2. Select the 3rd variable:                                                                                                                                                                                                                                                                                                                       `print(mydata [ , 3]); print(mydata[3]);`

  \\#3. Select the variables q1, q2 ,q3, q4  
`print(mydata[c(3,4,5,6)]); print(mydata[3:6])`

   \\#4. Exclude q1, q2 ,q3 q4  
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              `print(mydata[-c(3,4,5,6)]) ; print(mydata[-(3:6)])`

   \\#5. Using indices in a numeric vector:

`myQindices <-c (3,4,5,6); print(mydata[myQindices]) ]`

   \\#6.Display the indices for all variables:

```
print(data.frame(names(mydata)))
```

\#7. Using ncol to find the last index:  
`print(mydata[1:ncol(mydata)])`  
\# Python  
\# SAS

By Column name   
 R 

1. Display all variable names:                                                   

   names\(mydata\)                                                                       

 2. Select one variable :                                                               

  print\(mydata\["q1"\]\); \# pass q1 as a data frame;                   

  print\(mydata\[ , "q1"\]\) \# pass q1 as a vector                          

 3. Select several:                                                                        

  print\(mydata\[c\("q1", "q2", "q3", "q4"\)  \]\)                                 

 4. Save a list of variable names to use:                             

         myQnames &lt;- c\("q1", "q2", "q3", "q4"\) ; print \(mydata\[myQnames\]                                                                                           

 5. Generate a list of variable names:                                      

  myQnames &lt;- paste\("q", 1:4, sep = " "\) ; print\(mydata\[myQnames\]\) \|  
 Python    
SAS   
By logic   
1. Select q1 by entering TRUE/FALSE values:                            

print\(mydata\[c\(FALSE, FALSE, TRUE, FALSE, FALSE\)\]\)          

   2. Manually create a vector to get just q1:                               

 print\(mydata\[as.logial\(c\(0,0,1,0,0,0\)\)\]\)                               

        3. Automatically create a logical vector to get just q1:    

        print\(mydata\[names\(mydata\) == "q1"\]\)     or !names\(mydata\) == “q1"\]\)              

   4.Use the OR operator, "\|" to select q1 through q4:                

   myQtf &lt;- names\(mydata\) == "q1"\| names\(mydata\) == "q2"    

   print\(mydata\[myQtf\]\)                                

5. Use the %in% operator to select q1 through q4             

       myQtf &lt;- names\(mydata\) %in% c\("q1", "q2", "q3", "q4"\) \|  
Python   
 SAS 

 By string search   
 R  1. use grep to save the q variable indices:                  

             myQindices &lt;- grep\("^q", names\(mydata\), value = FALSE\)       

2. use grep to save the q variable names : value = TRUE       

  3. use %in% to create a logical vector                 

                       myQtf &lt;- names \(mydata\) %in% myQnames; print \|  
 Python 

 SAS   
By $notation   
R 1. print\(mydata$q1\)                                                                    

      print\(data.frame\(mydata$q1, mydata$q2\)       

                     2. by simple name: using attach function             

                     3. using "with" function                             

                                      with\(mydata, summary\(data.frame\(q1, q2, q3, q4\)\)\) \|  
 Python   
 SAS   
with subset function   
R 

1. print\(subset\(mydata, select = q1: q4\)\)                            

      2. print\(subset\(mydata, select = c\(workshop, q1:q4\) \|  
 Python   
SAS   
 Select variables by list subscript   
R

  print\(mydata\[\[3\]\]   
 Python   
SAS   
 Generate indices A to Z from two variables   
 R  

myqA &lt;- which\(names \(mydata\) == "q1"\)                         

          myqZ &lt;- which\(names\(mydata\) =="q4"\)                                 

    print\(mydata\[myqA : myqZ\) \|  
 Python   
SAS   
 Select numeric or character variables   
 R 

 1. is. numeric \(mydata$workshop\)                                            

      find numeric variables:                                                 

           2. mynums &lt;- sapply\(mydata, is.numeric\); print\(mydata \[myNums\]                   

                                                                          3. myA &lt;- which\(names\(mydata\) == "gender"\)         

                     myZ &lt;- which\(names\(mydata\) == "q3"\)                           

          myRange &lt;- 1 : length\(mydata\) %in% myA : myZ         

        print \(mydata\[myNums & myRange\]\)   
Python 

 SAS   
 Create a new data frame of selected variables 

  
 R 

myqs &lt;- mydata\[3:6\]                                                            

          myqs &lt;- mydata\[ c \("q1", "q2", "q3", "q4"\)\]                                  

 myqs &lt;- data.frame \(mydata$q1, mydata$q2, mydata$q3, mydata$q4\)            

  myqs &lt;- data.frame\(q1 = mydata$q1, q2 = mydata$q2, q3 = mydata$q3, q4= mydata $q4\)                           

  myqs &lt;- subset\(mydata, select = q1 :q4\)   
 Python   
SAS 



* 


