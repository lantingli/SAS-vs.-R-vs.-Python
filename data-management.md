\| Transforming variables \| R \| a. \#transformation in the middle of another function                       `summary(log(mydata$q4))`  
b. creating meanQ with dollar notation                 `mydata$meanQ <- mydata$q1 + mydata$q2 + mydata$q3 + mydata$q4)/4` \|  
\| :--- \| :--- \| :--- \|     
\|  \| Python \|  \|  
\|  \| SAS \| LIBNAME mylib 'C:\myRfolder'                 data mylib.mydatatransformed;                set mylib.mydata;                                    totalq = \(q1+q2+q3+q4\);                         logtot = log10\(totalq\)                              mean1 = \(q1+q2+q3+q4\)/4;                   mean2 = mean\(of q1 -q4\) \|  
\| PROCEDURES OR FUNCTIONS \| R \|  \|  
\|  \| Python \|  \|  
\|  \| SAS \|  \|  
\| CONDITIONAL TRANSFORMATION \| R \| a. the ifelse function                               b. cutting functions \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| MULTIPLE CONDITIONAL TRANSFORMATIONS \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| MISSING VALUES \| R \| a. substituting means for missing values                                                              b. finding complete observations         c. when "99" has meaning \|  
\|  \|  \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| RENAMING VARIABLES \| R \| a. renaming by index                               b. renaming by column name                 c. renaming many sequentially numbered variable names                                   d. renaming observations \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| RECORDING VARIABLES \| R \| a. recoding a few variables                     b. recoding many variables \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| INDICATOR OR DUMMY VARIABLES \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| KEEPING AND DROPPING VARIABLES \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| STACKING/CONCATENATING/ADDING DATA SETS \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| JOINING/MERGING DATA SETS \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| CREATING SUMMARIZED OR AGGREGATED DATSAETS \| R \| a. the aggregate function                       b. the tapply function                              c. merging aggregates with original data                                                              d. tabular aggregation                            e. the plyr and reshape2 pack \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| BY OR SPLIT -FILE PROCESSING \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| REMOVING DUPLICATE OBSERVATIONS \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| SELECTING FIRST OR LAST OBSERVATIONS PER GROUP \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| TRANSPOSING OR FLIPPING DATA SETS \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| RESHAPING VARIABLES TO OBSERVATIONS AND BACK \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| SORTING DATA FRAMES \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| CONVERTING DATA STRUCTURES \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| CHARACTER STRING MANIPULATIONS \| R \|  \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|  
\| DATES AND TIMES \| R \| a. calculating durations                           b. adding durations to date-time variables                                                            c. accessing date-time elements           d. creating date-time variables from elements                                                       e. logical comparisons with date-time variables                                                   f. formatting date-time output              g. two-digit years                                     h. date-time conclusion \|  
\|  \| PYTHON \|  \|  
\|  \| SAS \|  \|



|  |  |  |
| :--- | :--- | :--- |
|  |  |  |
|  |  |  |
|  |  |  |

|  | R | PYTHON | SAS |
| :--- | :--- | :--- | :--- |
| **1. Transforming variables** |  |  |  |
| **2. Procedures or Functions** |  |  |  |
| _ Applying the mean function_ |  |  |  |
| _Finding N or NVALID_ |  |  |  |
| _Standardizing and ranking variables_ |  |  |  |
| _ Applying your own functions_ |  |  |  |
| **3. Conditional Transformations** |  |  |  |
| _ifelse function_ |  |  |  |
| _Cutting functions_ |  |  |  |
| **4. Multiple conditional transformations** |  |  |  |
| **5. Missing values** |  |  |  |
| _Substituting means for missing values_ |  |  |  |
| _Finding complete observations_ |  |  |  |
| _when "99" has meaning_ |  |  |  |
| **6. Renaming variables\(and observations\)** |  |  |  |
| _Renaming by index_ |  |  |  |
| _Renaming by column names_ |  |  |  |
| _Renaming many sequentially numbered variable_ |  |  |  |
| _Renaming observations_ |  |  |  |
| **7. Recoding variables** |  |  |  |
| _Recoding a few variables_ |  |  |  |
| _Recoding many variables_ |  |  |  |
| **8. Indicator or dummy variables** |  |  |  |
| **9. Keeping and dropping variables** |  |  |  |
| **10. Stacking/Concatenating/Adding datasets** |  |  |  |
| **11. Joining/Merging data sets** |  |  |  |
| **12. Creating summarized or aggregated data sets** |  |  |  |
| _The aggregate function_ |  |  |  |
| _The tapply function_ |  |  |  |
| _Merging aggregates with original data_ |  |  |  |
| _Tabular aggregation_ |  |  |  |
| **13. By or split -file processing** |  |  |  |
| **14. Removing duplicate observations** |  |  |  |
| **15. Selecting first or last observations per group** |  |  |  |
| **16. Transposing or flipping data sets** |  |  |  |
| **17. Reshaping or flipping data sets** |  |  |  |
| **18. Sorting data frames** |  |  |  |
| **19. Converting data structure** |  |  |  |
| **20. Character string manipulations** |  |  |  |
| **21. Dates nad times** |  |  |  |
| _Calculating durations_ |  |  |  |
| _Adding durations ot date-time variables_ |  |  |  |
| _Accessing date-time elements_ |  |  |  |
| _Creating date-time variables from elements_ |  |  |  |
| _Logical comparisons with date-time variables_ |  |  |  |
| _Formatting date-time output_ |  |  |  |

1. 
2. Transforming variables: calculate mean

3. Mydata$meanQ &lt;- \(mydata$q1 +mydata $q2

   * mydata$q3+mydata$q4\)\/4

4. Mydata\[, “meanQ”\]&lt;- \(mydata \[,”q1”\] + mydata \[,”q2”\]

5. mydata \[,”q3”\] + mydata \[, “q4”\]\)\/4

6. Within\(mydata,

   meanQ &lt;- \(q1 +q2+q3+q4\)\/4

   \)

7. Mydata &lt;- transform\(mydata, meanQ = \(q1+q2+q3+q4\)\/4\)

8. Attach\(mydata\)

Mydata$meanQ &lt;- \(q1 + q2+q3+q4\)\/4

Detach\(mydata\)

1. Using plyr package

Mydata &lt;- data.frame\(cbind\(mydata, meanQ = 0.\)\)

Mydata \(7\) &lt;- \(mydata$q1 + mydata$q2

+mydata$q3+mydata$q4\)\/4

1. Procedures or functions

2. Calculate mean group by some variable

3. Mean\(mydata\[3:6\]. Na.rm = TURE\)

4. Mymatrix &lt;- mydata \[, 3:6\]

Mymatrix &lt;- as.matrix\(mydata\[3:6\]\)

Mydata$meanQ &lt;- Apply\(mymatrix, 2, mean, na,rm = TRUE\)

1: representing rows, 2 representing columns

1. Mydata$meanQ &lt;-rowMeans\(mymatrix, na.rm = TURE\)
2. Mydata&lt;- transform \(mydata, meanQ = rowMeans \(mymatrix, na.rm = TRUE\)
3. Lapply\(mydata\[ , 3:6\], mean, na.rm = TURE\)
4. Sapply\(mydata\[ , 3:6\], mean, na.rm = TRUE\)

5. Finding N or NVALID

6. Length\(mydata\[ , “q3”\]\): count the valid values of those variables for each observation

7. Nrow\(mydata\)

8. Sum\(!is.na\(mydata \[ , “q3”\]\)\)

9. Using prettyR package: sapply\(mydata, valid.n\)

10. Mymatrix &lt;- as.matrix\(mydata \[ , 3:6\]\)

Apply\(mymatrix, 1, valid.n\)

1. Mydata$myQn &lt;- apply\(mymatrix, 1, valid.n\)

2. Standardizing and ranking variables

myZs &lt;- apply\(mymatrix, 2, scale\) = \(sas: proc standard\)

myRanks &lt;- apply\(mymatrix, 2, rank\)

1. Applying your own functions

Apply\(mymatrix, 2, mean, sd\) \# not good.

Should write like this:

Mystats &lt;- function\(x\) {

C\(mean = mean\(x, na.rm = TRUE\),

Sd = sd \(x, na.rm = TRUE\)\)

}

Apply\(mymatrix, 2, mystats\)

Or：

Apply\(mymatrix, 2, function\(x\) {

C\(mean = mean \(x, na.rm = TRUE\),

Sd = sd \(x, na.rm = TRUE\)\)

} \)

1. Conditional transformation

2. The ifelse function

Mydata$q4agree&lt;- ifelse\(q4 ==5, 1,0\)

Mydata$q4agree&lt;- as.numeric\(q4 ==5\)

Mydata$ws1agree&lt;- ifelse\(workshop==1 & q4&gt;=4, 1, 0\)

Mydata$score &lt;- ifelse\(gender == “f”,

\(2\*q1\) +q2,

\(3\*q1\) +q2

\)

Postgroup &lt;-

Ifelse\(posttest&lt;60 ,1,

Ifelse\(posttest&gt;=60 & posttest &lt;70, 2,

Ifelse\(posttest&gt;=70 & posttest&lt;80, 3

Ifelse\(posttest &gt;= 80 & posttest &lt;90 , 4,

Ifelse\(posttest &gt;= 90 , 5, posttest\)

\)\)\)\)

1. Cutting functions

Use Hmisc package

Postgroup &lt;- cut2\(posttest, c\(60, 70, 80, 90\)\) \/ cut point

Postgroup &lt;- cut2\(posttest, g= 5\) \/ group number

Postgroup &lt;- cut2\(posttest, m = 25\)

Table \(postGroup\)

1. Multiple conditional transformations
2. Missing values

3. When importing numeric data, R reads blanks as missing\(except when blanks are delimiters\). R reads the string NA as missing for both numeric and character variables. When importing a text file, both SAS and SPSS would recognize a period as a missing value for numeric variables. R will, instead, read the whole variable as a character vector. If you have control over the source of the data, it is best not to write them out that way.

When other values represent missing, you will, of course, have to tell R about them. The read.table function provides an argument, na.strings, that allows you to provide a set of missing values.

mydataNA &lt;- read.table \(“mydataNA.txt”,

na.strings = c\(“.”, “9”, “99”\)\)

1. Substituting means for missing values

There are several methods for replacing missing values with estimates of what they would have been. These methods include simple mean substitution, regression and the gold standard – multiple imputation.

mydataNA$q1 \[is.na\(q1\)\]&lt;- mean\(q1, na.rm = TURE\)

1. Finding complete observations

2. Omit all observations that contain any missing values with na.omit function

Mynomissing &lt;- na.omit \(mydataNA\)

1. Get the cases that have no missing values\(the same result as the na.omit function\)

Mynomissing &lt;- mydataNA \[complete.cases \(mydataNA\),\]

1. When “99” has meaning

My9isNA &lt;- function\(x\) { x\[x==9\] &lt;- NA; x}

Or

My9isNA &lt;- function\(x\) { ifelse\(x ==9\), NA, x\) }

My99isNA &lt;- function\(x\){ifelse\(x==99\), NA, x\) }

mydataNA &lt;- read.table\(“mydataNA.txt”, na.strings = “.”\)

attach（mydataNA）

mydataNA\[3:4\] &lt;- lapply \(mydataNA\[3:4\], my9isNA\)

mydataNA\[5:6\] &lt;- lapply \(mydataNA\[5:6\], my99isNA\)

1. Renaming variables\(observations\)

2. Using reshape2 package

Mychanges &lt;- c\(q1 = “x1”, q2 = “x2”, q3 = “x3”, q4 = “x4”\)

Mydata &lt;- rename \(mydata, mychanges\)

1. Names\(mydata\) &lt;- c\(“workshop”, “gender”, “x1”, “x2”, “x3”, “x4”\)
2. Names\(mydata\) \[2\] &lt;- “sex”
3. Renaming by index

Mynames &lt;- names\(mydata\)

Data,frame\(mynames\)

Mynames\[3\] &lt;- “x1”

Mynames\[4\] &lt;- “x2”

Mynames\[5\]&lt;- “x3”

Mynames\[6\] &lt;-“x4”

1. Renaming by column name

Mynames &lt;- names\(mydata\)

Mynames \[mynames ==”q1”\] &lt;- “x1”

Mynames \[mynames ==”q2”\] &lt;- “x2”

Mynames \[mynames ==”q3”\] &lt;- “x3”

Mynames \[mynames ==”q4”\] &lt;- “x4”

Names\(mydata\) &lt;- mynames

1. Renaming many sequentially numbered variable names

myXs &lt;- paste \(“x”, 1:4, sep = “ “\)

myA &lt;- which\(names\(mydata\) ==”q1”\)

myZ&lt;-which\(names\(mydata\) ==”q4”\)

names\(mydata\) \[myA:myZ\] &lt;- myXs

1. Recoding variables

Recoding is just a simpler way of doing a set of related IF\/THEN conditional treansformation.

1. Using car package

Mydata$qr1 &lt;- recode\(q1, “1=2; 5=4”\)

Mydata$qr2 &lt;- recode\(q2, “1=2; 5=4”

Mydata$qr3 &lt;- recode\(q3, “1=2; 5=4”\)

Mydata$qr4 &lt;- recode\(q4, “1=2; 5=4”\)

1. Indicator or dummy variables

You can create indicator variables using the ifelse function or using the recode function, but the easiest approach is the same as in SAS and SPSS: using logic to generate a series of zeros and ones:

R&lt; - as.numeric \(workshop == “R”\)

Sas&lt;- as.numeric\(workshop ==”SAS”\)

Spss &lt;- as.numeric\(workshop = “SPSS”\)

Stata &lt;- as.numeric\(workshop ==”Stata”\)

Head\(data.frame\(

Workshop, r, sas, spss, stat\)\)

1. Keeping and dropping variables

Keep: myleft &lt;- mydata\[ , 1:4\]

Drop : mydata$varname &lt;- NULL

Myleft &lt;- myadta

Myleft$q3 &lt;- myleft$q4&lt;- NULL

NULL is only used to remove components from data frames and lists. You cannot use it to drop elements of a vector, nor can you use to remove a vector by itself from your workspace.

Rm\(mydata$q4\) this does not work, only remove whole objects, can not remove variables from within a data frame.

1. Stacking \/concatenating\/adding data sets

2. Rbind function: but rbind function needs both data frames to have the exact variable names.

3. Using plyr package

Both &lt;- rbind.fill\(females, males\) : binds whichever variables it finds that match and then fills in missing values for those that do not.

1. Joining \/merging data sets

Both &lt;- merge\(myleft, myright, by.x = “id”, by.y = “id”\)

Both &lt;- merge\(myleft, myright,

By = c\(“id”, “workshop”\)\)

Both &lt;- merge\(myleft, myright, by.x = c\(“id”, “workshop”\)

By.y = c\(“subject”, “shortcourse”\)）

By default, SAS and SPSS keep all records regardless of whether or not they match\(a full outer join\). For observations that do not have matches in the other file, the merge function will fill them in with missing values. R takes the opposite approach, keeping only those that have a record in both\(an inner join\). To get merge to keep all records, use the argument all = TURE. You can also use all.x = TRUE to keep all of the records in the first file regardless of whether or not they have matches in the second. The all.y = TRUE argument does the reverse.

While SAS and SPSS can merge any number of files at once, base R can only do two at a time. To do more, you can use the merge\_all function in the reshape package.

1. Creating summarized or aggregated data sets

2. Directly

Myagg1 &lt;- aggregate\(q1,

By = data.frame\(gender\),

Mean, na.rm = TRUE\)

Myagg2&lt;- aggregate\(q1,

By = data.frame\(workshop, gender\),

Mean, na.rm = TRUE\)

1. Using tapply function

The aggregate function has an important limitation: only use it with functions that return single values. The tapply function works very similarly to the aggregate function but without this limitation.

Myagg2&lt;- tapply\(q1,

Data.frame \(workshop, gender\),

Mean, na.rm = TRUE\)

1. Merging aggregates with original data
2. Tabular aggregation
3. The plyr and reshape2 packages

4. By or split-file processing

5. Removing duplicate observations

6. Completely duplicate observations

Mynoduplicates &lt;- unique \(myduplicates\) \/ unique function removed them but did not show them.

1. myDuplicates$DupRecs &lt;- duplicated \(myDuplicates\)

mynoduplicates &lt;- myduplicates \[!DupRecs, -7\]

1. duplicate keys

mykeys &lt;- c\(“workshop”, “gender”\)

mydata$dupkeys &lt;- duplicated \(mydata\[ , mykeys\]\)

1. Selecting first or last observations per group

myBys &lt;- data.frame\(mydata$workshop, mydata$gender\)

mylastlist &lt;- by\(mydata, myBys, tail, n =1\)

\#back into a data frame

mylastDF &lt;- do.call\(rbind, mylastList\)

another way to create the data frame:

mylastDF &lt;- rbind \(mylastList\[\[1\]\],

mylastList\[\[2\]\],

mylastList\[\[3\]\],

mylastList\[\[4\]\]\)

1. Transposing or flipping data sets

myQs &lt;- c\(“q1”, “q2”, “q3”, “q4”\)

myQdf &lt;- mydata \[ , myQs\]

myFlipped &lt;- t\(myQdf\)

myFixed &lt;- as.data.frame\(t\(myFlipped\)\)

1. Reshaping variables to observations and back
2. Sorting data frames

Mydata\[c\(4,3,2,1\), \]

myW &lt;- order \(mydata$workshop\)

mydata \[myW, \]

mydata\[rev\(myW\), \]

myDGW &lt;- order \(

-as.numeric \(mydata $gender\),

Mydata$workshop

\) \/ the default order is ascending, to reverse this, placet the minus sign before any variable. However, this only works with numeric variables, and we have been sorting by factors. We can use as.numeric function to extract the numeric values that are a behind – the –scenes oart of any factor.

1. Converting data structures

2. Character string manipulations

Stringr package

1. Dates and timesR



