### \\# 1. comma delimited files

SAS:
```
proc import out = mylib.mydata 
datafile = "c"\myRfolder\mydataID.csv" 
DBMS = CSV REPLACE; 
GETNAMES = YES; 
DATAROW = 2; 
RUN;
 
```
PYTHON:
R: 
\# with id variable not named
  `mydata <- read.csv("mydata.csv")`
  
\# with id named in the header
  `mydata <- read.csv("mydataID.csv", row.names = "id")`
  
### \\# 2. Tab delimited files:
SAS:
```
 PROC IMPORT OUT = mylib.mydata  
 DATAFILE = "C:\myRworkshop\mydataID.tab" 
 DBMS = TAB REPLACE; 
 GETNAMES = YES; 
 DATAROW =2; 
 RUN; 
```
PYTHON:
R:
\# read a tab delimited file with named ID columns

```
mydata <- read.delim("mydata.tab")
```

```
count.fields("mydata.tab", sep = "\t")
```

\# again with ID named in the header

```
mydata <- read.delim("mydataID.tab",row.names = "id")
```


### \\# 3. Reading from a web site: FILENAME myURL 
SAS:
```
FILENAME myURL URL "http://sites.google.com/site/r4statistics/mydataID.csv"; 

PROC IMPORT DATAFILE = myURL 
DBMS = CSV REPLACE 
OUT = mylib.mydata; 
GETNAMES = YES; 
DATAROW =2; 
RUN;
```
PYTHON:

R:
myURL <- "http:/sites.google.com/site/r4statistics/mydata.csv"
mydata<- read.csv(myURL)

\# reading text from the clipboard
     \# copy a column of numbers or words, then :
     `myvector <- readClipboard()`
     \# Open mydata.csv, select & copy contents, then :
     `mydata <- read.delim ("clipboard", header = TURE)`
     \# Missing values for character varables
     `mydata <- read.csv("mydataID.csv", row.names = "id", strip.white = TRUE, na.strings = "")`
     \# skipping variables in delimited text files
     `myCols <- read.delim("mydata.tab", strip.white = TRUE, na.strings = "", colClasses = c("integer", "integer", "character", "NULL", "NULL", "integer", "integer"))`

## \\#4. reading text data within a program
R:
1. the stdin approach
```
mydata <- read.csv(stdin())
 workshop, gender, q1, q2, q3, q4 
 1,1,f,1,1,5,1 
```
2. blank line above ends input 

3. the testConnection approach 

```
mystring <- "workshop, gender, q1, q2, q3, q4 
1, 1, f,1 1, 1, 5, 1" 
mydata <- read.csv(textConnection(mystring))
```

PYTHON:

SAS
```
LIBNAME myLib 'C:\myRfolder'; 
DATA mylib.mydata; 
INFILE DATALINES DELIMITER = ',' 
MISSOVER DSD firstobs = 2; 
INPUT id workshop gender $ q1 q2 q3 q4; DATALINES; 
id, workshop, gender, q1 q2, q3, q4 1,1,f,1,1,5,1 
PROC PRINT; 
RUN;
```

### \\# 5. Reading multiple observations per line
R:

```
1. mylist <- scan(stdin(), 

```
what = list(id = 0, workshop = 0, gender = " ",
 q1 =0, q2 = 0, q3, = 0, q4 = 0)) 
 1 1 f 1 1 5 1 
```

2. Blank line above ends input mydata <- `data.frame(mylist) `
3. the textConnection approach 

```
mystring <- "1 1 f 1 1 5 1"; 
mylist <- scan(textConnection(mystring), 
what = list(id =0, workshop = 0, gender = " ", 
q1 =0, q2 = 0, q3 = 0, q4 =0 )) 
mydata <- data.frame(mylist)
```
PYTHON:

SAS:
```
DATA mydata; 
INPUT id workshop gender $q1 - $q4 @@; 
DATALINES; 1 1 f 1 1 5 1 ; 
PROC PRINT; 
RUN;

```
### \\# 6. Reading fixed-width text files : one record per case



```
1. mydata <- read.fwf( 


```
file = "mydataFWF.txt",
 width = c(2, -1, 1, 1, 1,1,1 ), 
 col.names = c("id", "gender", "q1", "q2", "q3", "q4"), row.names = "id", 
 na.strings = "", 
 fill = TRUE, 
 strip.white = TRUE) 
```
2. using "macro substitution".


```
myfile <- "mydataFWF.txt" 
myvariablenames <- c("id", "gender", "q1", "q2", "q3", "q4")
 myvariablewidths <- c(2, -1, 1,1,1,1,1) 
 mydata <- read.fwf( file = myfile, width = myVariableWidths, col.names = myVariableNames, 
 row.names = "id", na.strings = " ", fill = TRUE, strip.white = TRUE)
```

PYTHON:

SAS
```
LIBNAME mylib 'C:\myRolder'; 
DATA myLib.mydata; 
INFILE '\myRfolder\mydataFWF.txt' MISSOVER; 
INPUT ID 1-2 WORKSHOP 3 GENDER $ 4 q1 5 q2 6 q3 7 q4 8; 
RUN;
```
### \\# 7. Reading fixed - width text files, two or more records per case

```
myfile <- "mydataFWF.txt" 
myVariableNames <- c ("id", "workshop", "gender", "q1", "q2", "q3", "q4", "q5", "q6", "q7", "q8") 
myRecord1Widths <- c(2,1,1,1,1,1,1) 
myRecord2Widths <- c（-2， -1， -1， 1，1，1，1） 
myVariableWidths <- list(myRecord1Widths, myRecord2Widths) 
mydata <- read.fwf( file = myfile, width = myVariableWidths, col.names = myVariableWidths, row.names = "id", na.strings = " ", fill = TRUE, strip.white = TRUE)
```
SAS:
```
DATA temp; 
INFILE '\myRfolder\mydataFWF.txt' MISSOVER; 
INPUT \#1 id 1-2 workshop 3 gender 4 q1 5 q2 6 q3 7 q4 8 \#2 q5 5 q6 6 q7 7 q8 8; 
PROC PRINT; 
RUN;
```
### \\# 8. reading excel files
R: 

```
library("xlsReadWrite") 
xls.getshlib():
```

\#can get a binary file that is not distributed through CRAN 

```
mydata <- read.xls("mydata.xls")
```
PYTHON:

SAS:

```
LIBNAME mylib "c:\myRfolder"; 
PROC IMPORT OUT = mylib.mydata 
DATAFILE = "C:\myRfolder\mydata.xls" 
DBMS = EXCELCS REPLACE; 
RANGE = "Sheet1$"; 
SCANTEXT = YES; 
USEDATE = YES; 
SCANTIME = YES;
run;
```

### \\# 9. Reading from relational databases

```
library("RODBC") 
myConnection <- odbcConnectExcel("mydata.xls") 
mydata <- sqlFetch(myConnection, "Sheet1") 
close(myConnection)
```
### \\# 10. Reading data from SAS

```
library("foreign")   
mydata <- read.ssd("c:/myRfolder", "mydata",   
sascmd - "C:/Program files/SAS/SASFoundation/9.2/sas.exe")                                                                                                  
library("Hmisc")                                                                                                                                            
mydata <- sasxport.get("mydata.xpt")                                                                                                                                                                             
```

### \\# 11. write data from SAS and read it into R



```
LIBNAME mylib 'C:\myRfolder'; 
LIBNAME To\_R xport '\myRfolder\mydata.xpt'; 
DATA To_R.mydata; 
set mylib.mydata; 
RUN; 
\#\# read a SAS data set 
\# read ssd or sas7bdat if you have SAS installed
library("foreign") 
mydata <- read.ssd("c:/myRfolder“， ”mydata", sascmd = "C:/program files /SAS/SASFoundation/9.2/sas.exe") 
\# reads SAS export format without installing SAS library("foreign") 
library("Hmisc") 
mydata <- sasxport.get("mydata.xpt")
```



### \\# 12. Writing delimited text files

SAS: 

```
PROC PRINT DATA = mylib.mydata; 
run; 
PROC EXPROT DATA = mylib.mydata 
outfile = "C"\myFolder\mydataFromSAS.csv" 
DBMS = CSV REPLACE; 
PUTNAMES = YES; 
RUN; 
PROC EXPORT DATA = mylib.mydata 
outfile = "C"\myFolder\mydataFromSAS.txt" 
DBMS = TAB REPLACE; 
PUTNAMES = YES; 
RUN;
```

R:

```
1. write.csv(mydata, "mydataFromR.csv") 
2. write.table(mydata, "mydataFromR.txt") 
3. write.table(mydata, file = "mydataFromR.txt", 
quote = FALSE, sep = "\t", na = " ", 
row.names = TRUE, col.names = TRUE)
```

### \\# 13. Viewing a text fileViewing a text file

```
file.show("mydataFromR.csv")
```
### \\# 14. Writing Excel files
R:

```
library("xlsReadWrite")
xls.getshlib() 
load("mydata.RData") 
write.xls (mydata, "mydataFromR.xls")
```
SAS: 
```
LIBNAME mylib "c:\myFolder"; 
PROC EXPORT DATA = mylib.mydata 
OUTFILE = "C:\myFolder\mydata.xls" 
DBMS = EXCELCS LABEL REPLACE; 
SHEET = "mydata"; 
RUN;
```
### \\# 15. Writing to relational databases

```
library("RODBC") 
myConnection <- odbcConnectExcel("mydataFromR.xls", readOnly = FALSE) 
sqlSave(myConnection, mydata) 
close(myConnection)
```
### \\# 16. wirting data to SAS

```
library("foreign") 
write.foreign (mydata, datafile = "mydataFromR.csv", codefile = "mydata.sas", package = "SAS")
```

# PYTHON

# Data loading, storage, and file formats

Input and output typically falls into a few categories: reading text files and other more efficient on-disk formats, loading data from databases, and interacting with network sources like web APIs.

Parsing functions in pandas

Read\_csv： loading delimited data from a file, URL, or file-like object. Use comma as default delimiter

Read\_table: loading delimited data from a file, URL, or file-like object. Use tab \(‘\t’\) as fault delimiter.

Read\_rwf: read data in fixed-width column format \(that is, no delimiters\)

Read\_clipboard: version of read\_table that reads data from the clipboard. Useful for converting tables from web pages.

These function are meant to convert text data into a data frame. The options for these functions fall into a few categories:

Indexing: can treat one or more columns as the returned data frame, and whether to get column names from the file, the user, or not at all.

Type inference and data conversion: this includes the user-defined value conversions and custom list of missing value markers.

Datetime parsing: includes combining capability, including combining date and time information spread over multiple columns into a single column in the result.

Iterating: support for iterating over chunks of very large files.

Uncleaning data issues: skipping rows or a footer, comments, or other minor things like numeric data with thousands separated by commas.

Type inference is one of the more important features of these functions; that means you do not have to specify which columns are numeric, integer, Boolean, or string. Handling dates and other custom types requires a bit more effort though.

1\) example:

Df = pd.read\_csv\(‘ch06\/ex1.csv’\)

Pd.read\_table\(‘ch06\/ex1.csv’, sep = ‘,’\) \/\/ specifying the delimiter

Pd.read\_csv\(‘ch06\/ex2.csv’, header = None\)

Pd.read\_csv\(‘ch06\/ex2.csv’, names = \[‘a’, ‘b’, ‘c’, ‘d’, ‘message’\]\)

Suppose you wanted the message column to be the index of the returned dataframe. You can either indicate you want the column at index 4 or named ‘message’ using the index\_col argument:

Names = \[‘a’, ‘b’, ‘c’, ‘d’. ‘message’\]

Pd.read\_csv\(‘ch06\/ex2.csv’, names = names, index\_col = ‘message’\)

In the event that you want to form a hierarchical index from multiple columns, just pass a list of column numbers or names:

Parsed = pd.read\_csv\(‘ch06\/csv\_mindex.csv’, index\_col = \[‘key1’, ‘key2’\]\)

In some cases, a table might not have a fixed delimiter, using whitespace or some other pattern to separate fields. In these cases, you can pass a regular expression as a delimiter for read\_table.

Result = pd.read\_table \(‘ch06\/ex3.txt’, sep = ‘\s+’\)

Pd.read\_csv \(‘ch06\/ex4.csv’, skiprows = \[0,2,3\]\) \/\/ skip the rows

Result = pd.read\_csv\(‘ch06\/ex5.csv’, na\_values = \[‘NULL’\]\)

1. reading text files in pieces

pd.read\_csv\(‘ch06\/ex6.csv’, nrows = 5\)

chunker = pd.read\_csv\(‘ch06\/ex6.csv’, chunksize = 1000\)

1. writing data out to text format

data.to\_csv\(‘ch06\/out.csv’\)

data.to\_csv\(sys.stdout, sep = ‘\|’\)

data.to\_csv\(sys.stdout, na\_rep = ‘NULL’\)

data.to\_csv\(sys.stdout, index = false, header = false\)

data.to\_csv\(sys.stdout, index = false, cols = \[‘a’, ‘b’, ‘c’’\]\)

1. for series:

dates = pd.date\_range\(‘1\/1\/2000’, periods = 7\)

ts = series \(np.arrange\(7\), index = dates\)

ts.to\_csv\(‘ch06\/tseries.csv’\)

1. Reading Microsoft excel files

Xls\_file = pd.ExcelFile\(‘data.xls’\)

Table = xls\_file.parse\(‘Sheet1’\)

# SAS



