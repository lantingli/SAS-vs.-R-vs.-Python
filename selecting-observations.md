# R

|  | Selecting observations by index |
| :--- | :--- |
| R | 1. print all rows                                                                              print\(mydata \[ \]\)  ; print\(mydata\[, \]\); print\(mydata\[1:8\]\)            2. Just observation 5: print\(mydata\[5, \]\)                                    3. multiple: print\(mydata \[ c\(5,6,7,8\), \]\)                                      4. use indice:                                                                                 myMindices &lt;- c\(5,6,7,8\)  ; summary\(mydata\[myMindices,  \]\)                                                                                                     5. print a list of index numbers for each observation               data.frame\(myindex = 1:8, mydata\)                                          6. Select data using length as the end:                                      print\(mydata\[1:nrow\(mydata\), \]\) |
| Python |  |
| SAS |  |
|  | By row name |
|  | 1. Display row names: row.names \(mydata\)                              print\(mydata \[ c\("1", "2", "3", "4"\), \]\)                                             2. Assign new names:                                                                  mynames &lt;- c\("Ann", "Cary", "Sue"\)  ; row.names\(mydata\) &lt;- mynames                                                                                       3. print\(mydata\["Ann", \]\)                                                               4. print\(mydata \[c\("Ann", "Cary", "Sue", "Carla"\), \]\)                     5. myNames &lt;- c\("Ann", "Cary", "Sue"\); print\(mydata\[mynames, \]\) |
| Python |  |
| SAS |  |
|  | By Logic |
| R | 1. myRows &lt;- c\(TRUE, TRUE, TRUE, TRUE, FALSE, FALSE, FALSE, FALSE\)   ; print\(mydata\[myRows,\]\)                                        2. select first four rows using 1 and 0:                                       myBinary &lt;- c\(1,1,1,1,0,0,0,0\) ; print\(mydata\[myBinary, \]\)         myRows &lt;- as.logical\(myBinary\) ; print\(mydata\[myRows, \]\)    3. print\(mydata\[mydata$gender == "f", \]\)                                       print\(mydata\[which \(mydata$gender == "f"\), \]\)                     4. using a saved vector                                                                myFemales &lt;- which\(mydata$gender == "f” ）                         print\(mydata\[myFemales, \]\)                                                        5. using %in%                                    |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |



