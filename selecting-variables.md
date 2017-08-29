# R

### 查询变量的类型

常用的查询变量类型的函数有: mode, storage.mode, class 和 typeof. 这些函数有一些差别.

* storage.mode 是数据实际在内存中存储所采用的方式.
* class 是面向对象的 R, 比如说 data.frame 实际上的存储方式 \(storage.mode\) 是 list, 但是为了更好处理表单数据, 就包装成为了 data.frame 类型.
* mode 和 typeof 给出的结果很接近, 是实际上的类型, 但是在 mode 中, "integer" 和 "double" 都被认为是 "numeric".

如果需要知道环境中每个基本类型能存储的大小, 可以查询 `.Machine` 这个 list, 相应的大小存储在该 list 中.

### NA

由于种种原因, 数据中可能会出现缺失值的情况, R 会用 NA 来替代相应的空值. 如果是计算后产生的空值, 则会用 Inf 或者 NaN 来代替. 处理空值是数据分析中的必要部分.

* 判断 NA 可以使用 is.na 或者 is.nan 函数, 其中 is.na 会把 NA, Inf, NaN 都认为是 NA, 而 is.nan 则只关注 NaN.
* mean, var, sum, min, max, 等函数都有 na.rm 参数, 设置成真, 则会在计算的时候把 NA 给除去.
* lm, glm, gam 等函数有 na.action 参数, 该参数接受函数作为变量, 如 na.omit, na.fail. na.pass, na.exculde 等.
* na.omit 和 complete.cases 都可以返回一个只包含完整数据行的 data.frame, 也就是说如果一行中有一个或多个 NA, 该行就会被剔除.
* 对于 read.table 等函数可以使用 na.strings 可以把特定的数值或字符认为是 NA.

* **Selecting variables**


by index number 

\#1. Select all variables by default:   
                                                  


```
print(mydata[ ]); print (mydata[, ]);
```



\# 2. Select the 3rd variable:     

```                                                                                                   print(mydata [ , 3]); print(mydata[3]); 
```                                                                                                                                                                                                                                       \#3. Select the variables q1, q2 ,q3, q4     
```                                                                                                                                                                              print(mydata[c(3,4,5,6)]); print(mydata[3:6])   
```                                                                                                                                                                                   \#4. Exclude q1, q2 ,q3 q4   
 ```                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               print(mydata[-c(3,4,5,6)]) ; print(mydata[-(3:6)]) 
 ```                                                                                                                                                                                                                                                                                     
\#5. Using indices in a numeric vector: 
 ```
 myQindices <-c (3,4,5,6); print(mydata[myQindices])  
 ```          
\#6.Display the indices for all variables: 
```
print(data.frame(names(mydata))) 
```
\#7. Using ncol to find the last index: 
```
print(mydata[1:ncol(mydata)]) 
```
| Python |  |
| SAS |  |
|  | By Column name |
| R | 1. Display all variable names:                                                      names\(mydata\)                                                                            2. Select one variable :                                                                 print\(mydata\["q1"\]\); \# pass q1 as a data frame;                     print\(mydata\[ , "q1"\]\) \# pass q1 as a vector                              3. Select several:                                                                          print\(mydata\[c\("q1", "q2", "q3", "q4"\)  \]\)                                      4. Save a list of variable names to use:                                      myQnames &lt;- c\("q1", "q2", "q3", "q4"\) ; print \(mydata\[myQnames\]                                                                                                 5. Generate a list of variable names:                                        myQnames &lt;- paste\("q", 1:4, sep = " "\) ; print\(mydata\[myQnames\]\) |
| Python |  |
| SAS |  |
|  | By logic |
|  | 1. Select q1 by entering TRUE/FALSE values:                            print\(mydata\[c\(FALSE, FALSE, TRUE, FALSE, FALSE\)\]\)             2. Manually create a vector to get just q1:                                print\(mydata\[as.logial\(c\(0,0,1,0,0,0\)\)\]\)                                       3. Automatically create a logical vector to get just q1:            print\(mydata\[names\(mydata\) == "q1"\]\)     or !names\(mydata\) == “q1"\]\)                                                                                     4.Use the OR operator, "\|" to select q1 through q4:                   myQtf &lt;- names\(mydata\) == "q1"\| names\(mydata\) == "q2"       print\(mydata\[myQtf\]\)                                                                  5. Use the %in% operator to select q1 through q4                    myQtf &lt;- names\(mydata\) %in% c\("q1", "q2", "q3", "q4"\) |
| Python |  |
| SAS |  |
|  | By string search |
| R | 1. use grep to save the q variable indices:                               myQindices &lt;- grep\("^q", names\(mydata\), value = FALSE\)       2. use grep to save the q variable names : value = TRUE         3. use %in% to create a logical vector                                        myQtf &lt;- names \(mydata\) %in% myQnames; print |
| Python |  |
| SAS |  |
|  | By $notation |
| R | 1. print\(mydata$q1\)                                                                          print\(data.frame\(mydata$q1, mydata$q2\)                            2. by simple name: using attach function                                  3. using "with" function                                                                   with\(mydata, summary\(data.frame\(q1, q2, q3, q4\)\)\) |
| Python |  |
| SAS |  |
|  | with subset function |
| R | 1. print\(subset\(mydata, select = q1: q4\)\)                                  2. print\(subset\(mydata, select = c\(workshop, q1:q4\) |
| Python |  |
| SAS |  |
|  | Select variables by list subscript |
| R | print\(mydata\[\[3\]\] |
| Python |  |
| SAS |  |
|  | Generate indices A to Z from two variables |
| R | myqA &lt;- which\(names \(mydata\) == "q1"\)                                   myqZ &lt;- which\(names\(mydata\) =="q4"\)                                     print\(mydata\[myqA : myqZ\) |
| Python |  |
| SAS |  |
|  | Select numeric or character variables |
| R | 1. is. numeric \(mydata$workshop\)                                                  find numeric variables:                                                            2. mynums &lt;- sapply\(mydata, is.numeric\); print\(mydata \[myNums\]                                                                                             3. myA &lt;- which\(names\(mydata\) == "gender"\)                              myZ &lt;- which\(names\(mydata\) == "q3"\)                                     myRange &lt;- 1 : length\(mydata\) %in% myA : myZ                 print \(mydata\[myNums & myRange\]\) |
| Python |  |
| SAS |  |
|  | Create a new data frame of selected variables |
| R | myqs &lt;- mydata\[3:6\]                                                                      myqs &lt;- mydata\[ c \("q1", "q2", "q3", "q4"\)\]                                   myqs &lt;- data.frame \(mydata$q1, mydata$q2, mydata$q3, mydata$q4\)                                                                                       myqs &lt;- data.frame\(q1 = mydata$q1, q2 = mydata$q2, q3 = mydata$q3, q4= mydata $q4\)                                                     myqs &lt;- subset\(mydata, select = q1 :q4\) |
| Python |  |
| SAS |  |

* 


