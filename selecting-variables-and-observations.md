| SAS | data mylib.mymalewq;                                                                    set mylib.mydata;                                                                        where gender = "m";                                                                      keep workshop q1 -q4;                                                            run; |
| :--- | :--- |
|  | the subset function |
| R | myMalesWQ &lt;-  subset\(mydata,                                                                                  subset = gender == "m",                                                               select = c\(workshop, q1 :q4\)\) |
| Python |  |
|  | logic for obs, names for vars |
| R | myMales &lt;- which\(gender == "m"\)                                              myVars &lt;- c\("workshop", paste\(q, 1:4, sep = ""\)\)                       myMalesWQ &lt;- mydata\[myMales, myVars\] |
| Python |  |
|  | Row and Variable names |
| R | myMales &lt;- c\("5", "6", "7", "8"\)                                                       myVars &lt;- c\("workshop", "q1", "q2", "q3", "q4"\) |
| Python |  |
|  | Numeric index vectors |
| R | myMales &lt;- c\(5,6,7,8\)                                                                   myVars &lt;- c\(1,3,6\)                                                                         print\(mydata\[myMales, myVars\) |
| Python |  |
|  | Saving and loading subsets |
| R | myMalesWQ &lt;- subset\(mydata, subset = gender == "m", select = c\(workshop, q1 :q4\)                                                               save\(mydata, myMalesWQ, file = "myboth.RData"\)                    load\("myBoth.RData"\)                                                                 save \(myMalesWQ, file = "myMalesWQ.RData"\)                         load\("myMalesWQ.RData\) |
| Python |  |
|  |  |





SAS:

data mylib.mymalewq; set mylib.mydata; where gender = "m"; keep workshop q1 -q4; run;

\\# the subset function

myMalesWQ &lt;- subset\(mydata, subset = gender == "m", select = c\(workshop, q1 :q4\)\)

\\#logic for obs, names for vars

myMales &lt;- which\(gender == "m"\) myVars &lt;- c\("workshop", paste\(q, 1:4, sep = ""\)\) myMalesWQ &lt;- mydata\[myMales, myVars\]

\\# row and variable names

myMales &lt;- c\("5", "6", "7", "8"\) myVars &lt;- c\("workshop", "q1", "q2", "q3", "q4"\)

\\# numeric index vectors

myMales &lt;- c\(5,6,7,8\) myVars &lt;- c\(1,3,6\) print\(mydata\[myMales, myVars\)

\\# saving and loading subsets

myMalesWQ &lt;- subset\(mydata, subset = gender == "m", select = c\(workshop, q1 :q4\) save\(mydata, myMalesWQ, file = "myboth.RData"\) load\("myBoth.RData"\) save \(myMalesWQ, file = "myMalesWQ.RData"\) load\("myMalesWQ.RData\)



















