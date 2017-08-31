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
| R | 1. myRows &lt;- c\(TRUE, TRUE, TRUE, TRUE, FALSE, FALSE, FALSE, FALSE\)   ; print\(mydata\[myRows,\]\)                                        2. select first four rows using 1 and 0:                                       myBinary &lt;- c\(1,1,1,1,0,0,0,0\) ; print\(mydata\[myBinary, \]\)         myRows &lt;- as.logical\(myBinary\) ; print\(mydata\[myRows, \]\)    3. print\(mydata\[mydata$gender == "f", \]\)                                       print\(mydata\[which \(mydata$gender == "f"\), \]\)                     4. using a saved vector                                                                myFemales &lt;- which\(mydata$gender == "f” ）                         print\(mydata\[myFemales, \]\)                                                        5. using %in%                                                                                myRsas &lt;- which\(mydata$workshop %in% c\("R", "SAS"\)\);  print \(myRsas\) ; print\(mydata\[myRsas, \]\)                                        6. equvalent selections using different ways to refer to the variables                                                                                             a. print\(subset\(mydata, gender == 'f'\)   \)                                   b. attach \(mydata\)                                                                             print\(mydata\[which\(gender == 'f' \), \]\)                                        detach\(mydata\)                                                                       c. with\(mydata, print \(mydata\[which\(gender == 'f'\), \]\)\)            d. print\(mydata \[which\(mydata\['gender'\] == "f"\), \]\)                    e. print\(mydata\[which\(mydata$gender =="f"\), \]\)  |
| Python |  |
| SAS |  |
|  | by string search |
| R | \# search for row names that begin with “C”                               myCindices &lt;- grep \("^C", row.names \(mydata\), value = FALSE\)                                                                                                     print \(mydata\[myCindices , \]\)  |
| Python |  |
| SAS |  |
|  | by subset function |
| R | subset\(mydata, subset = gender == "f"\)                                      summary\(                                                                                         subset\(mydata, subset = gender = "m" & q4 ==5\)                   ） |
| Python |  |
| SAS |  |
| R | Generating indices A to Z from two row names                           myMaleA &lt;- which\(row.names\(mydata\) == "Bob"\)                    myMaleZ &lt;- which\(row.names\(mydata\) == "Rich"\)                  print\(mydata\[myMaleA :myMaleZ, \]\)  |
| R | Creating a new data frame of selected observations                  a. myMales &lt;- mydata \[5:8, \]                                                        b.  myMales &lt;- mydata\[which\(mydata$gender =="m"\), \]          c. myMales &lt;- subset\(mydata, subset = gender == "m"\)            |



