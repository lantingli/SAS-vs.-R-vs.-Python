## 1. Comma delimited files

### **SAS:**

```
proc import out = mylib.mydata 
datafile = "c"\myRfolder\mydataID.csv" 
DBMS = CSV REPLACE; 
GETNAMES = YES; 
DATAROW = 2; 
RUN;
```

### **PYTHON:**

read\_csv: load delimited data from a file, URL, or file-like object.use comma as default delimiter;  
read\_table: load delimited data from a file, URL, or file-like object. use tab\('\t'\) as default delimiter;  
read\_fwf: read data in fixed-width column format\(that is, no delimiters\);  
read\_clipboard: version of read\_table that reads data from the clipboard. useful for converting tables from web pages.

```

```

or

```
df = pd.table('ch06/ex1.csv', sep =",")
```

f= pd.read\_csv\('ch06/ex1.csv'， index\_col = 0\) \# pass the column number or column name you wish to use as the index  
pd.read\_csv \('foo.csv', index\_col = 'date'\)  
pd.read\_csv\('foo.csv', index\_col = \[0,'A'\]\)\# hierarchical index

# 1. for dialect keyword: gives greater flexibility in specifying the file format, uses the excel dialect by default

data = 'a,b,c~1, 2,3~4, 5,6'  
pd.read\_csv\(StringIO\(data\), lineterminator = '~'\)

or another common dialect option  
data = 'a,b,c\n1,2,3\n4,5,6'  
pd.read\_csv\(StringIO\(data\), skipinitialspace = True\)

# 2. specifying column data types: you can indicate the data type for the whole dataframe or individual columns

data = 'a, b, c\n1, 2,3\n4, 5,6\n7, 8,9'  
df = pd.read\_csv\(StringIO\(data\), dtype = object\)  
or  
df = pd.read\_csv\(StringIO\(data\), dtype = 'b': object, 'c': np.float64\)\)

# 3. specifying categorical dtype

data = 'col1, col2, col3\na, b, 1\na, b, 2\nc, d, 3'  
pd.read\_csv\(StringIO\(data\), dtype= 'category'\).dtypes

or for individual column:  
pd.read\_csv\(StringIO\(data\), dtype= 'col1': 'category'\)\).dtypes

if the categories are numeric they can be converted using the to\_numeric \(\) function or the other appropriate converter such as to\_datetime\(\)  
df= pd.read\_csv\(StringIO\(data\), dtype = 'category'\)  
df\[col3'.cat.categories = pd.to\_numeric\(df\['col3'\].cat.categories\)

# 4. Naming and Using columns: a file may or may not have a header row, pandas assumes the first row should be

data = 'a,b,c\n1,2,3\n4,5,6\n7,8,9'

pd.read\_csv\(StringIO\(data\), names = \['foo', 'bar', 'baz'\], header = 0\) \# throw away the header  
pd.read\_csv\(StringIO\(data\), names = \['foo', 'bar', 'baz'\], header = None\) \# keep the raw header

pd.read\_csv\(StringIO\(data\), header = 1\) \# if the header is in a row other than the first, pass the row number to header

# 5. Duplicate names parsing: if the file or header contains duplicate names, pandas by default will deduplicate these names so as to prevent data overwrite:

data = 'a,b', a\n0,1,2\n3,4,5'

pd.read\_csv\(StringIO\(data\), mangle\_dupe\_cols = False\) \# will arise duplicate data, so a value error will report

# 6. Filtering columns: the usecols argument allows to select any subset of the columns in a file, either using the column names or position numbers:

data = 'a,b,c,d\n1,2,3,foo\n4,5,6,bar\n7,8,9,baz'

pd.read\_csv\(StringIO\(data\), usecols = \['b', 'd'\]\)  
pd.read\_csv\(StrinIO\(data\), usecols = \[0,2,3\]\)

# 7. Comments and Empty lines:

Ignoring line comments and empty lines

data = '\na, b,c\n, \n\# commented line\n1,2,3\n\n4,5,6'

pd.read\_csv\(StringIO\(data\), comment= '\#'\)

data = 'a,b,c\n\n1,2,3\n\n\n4,5,6'

pd.read\_csv\(StringIO\(data\), skip\_blank\_lines = False\)

data = '\#comment\na,b,c\nA,B,C\n1,2,3'

pd\_read\_csv\(StringIO\(data\), comment = '\#', skiprows = 2\)

comments: if the comments or meta data included in a file

df = pd.read\_csv\('tmp.csv', comment = '\#'\)

\#8. dealing with unicode data

df = pd.read\_csv\(BytesIO\(data\), encoding = 'latin-1'\)

\#9 Index columns and trailing delimiters

if a file has one more column of data than the number of column names, the first column will be used as the dataframe's row names:

data = 'a,b,c,\n4,apple,bat,5.7\n8,orange,cow,10'

pd.read\__csv\(StringIO\(data\), index\__col = 0\)

pd.read_csv\(StringIO\(data\), index_col = False\)



\#10. Date Handling

specifying data columns

To better facilitate working with datatime data, read_csv\(\) and readtable\(\) use the keyword arguments parse\_dates and data\_parser to allow users to specify a variety of columns and date/time formats to turn the input text data into datetime objects_

df = pd.read\__csv\('foo.csv', index\_col = 0, parse\_dates = True\)_

df = pd.read\__csv\('tmp.csv', header = None, parse\_dates = \[\[1,2\], \[1,3\]\]\) \# for concatenation of the component column names_

df = pd.read\__csv\('tmp.csv', header = None, parse\_dates = \[\[1,2\], \[1,3\]\], keep\_date\_col = True\) keep raw date column_

date\_spec = \('nominal':\[1,2\],  'actural': \[1,3\]\)

df = pd.read_csv\('tmp.csv', header = None, parse_dates = date\_spec\) \#specify new column names

df = pd.read_csv\('tmp.csv', header = None, parse\_dates = date\_spec, date\_parser = conv.parse\_date\_time\) \# since read\_csv has a fast path for parsing datetime strings in iso8601 format, so if we can arrange for our data to store datetimes in this format, load times will be significantly faster. _

 Infering datetime format

df = pd.read\__csv\('foo.csv', index\__col = 0, parse\__dates = True, infer\_datetime\_format = True\)_

International date formats

while US date formats tend to be MM/DD/YYYY, many international formats use DD/MM/YYYY instead, a dayfirst keyword is provided, so always:

pd.read\__csv\('tmp.csv', dayfirst = True, parse\_dates = \[0\]\)_

\#11. Specifying method for floating -point conversion

pd.read\_csv\(















**a. Assign column names**

```
pd.read_csv('ch06/ex2.csv', names  = ['a','b', 'c', 'message'])
```

### **b. Assign the index **

```
names = ['a', 'b', 'c', 'd', 'message']
pd.read_csv('ch06/ex2.csv', names = names, index_col = 'message')  # or can assign two index_col
```

### **c. Skip specified rows**

```
pd.read_csv('ch06/ex4.csv', skiprows = [0, 2,3])
```

### **d. Handling missing values**

```
result = pd.read_csv('ch06/ex5.csv', na_values = ['NULL'])
```

Different NA sentinesl can be specified for each column in a dict:

```
sentinels = {'message': ['foo', 'NA'], 'something': ['two']}
pd.read_csv('ch06/exe5.csv', na_values = sentinels)
```

### **e. Only read out a small number of rows**

```
pd.read_csv('ch06/ex6.csv', nrows = 5)
```

### ** R: **

#### ** a. With id variable not named**

`mydata <- read.csv("mydata.csv")`

#### ** b. With id named in the header**

`mydata <- read.csv("mydataID.csv", row.names = "id")`

## 2. Tab delimited files:

### **SAS:**

```
 PROC IMPORT OUT = mylib.mydata  
 DATAFILE = "C:\myRworkshop\mydataID.tab" 
 DBMS = TAB REPLACE; 
 GETNAMES = YES; 
 DATAROW =2; 
 RUN;
```

### **PYTHON:\(NEED TO CONFRIM\)**

### **R:**

#### **a. Read a tab delimited file with named ID columns**

```
mydata <- read.delim("mydata.tab")
```

```
count.fields("mydata.tab", sep = "\t")
```

#### **b. With ID named in the header**

```
mydata <- read.delim("mydataID.tab",row.names = "id")
```

## 3. Reading from a web site: FILENAME myURL

### **SAS:**

```
FILENAME myURL URL 

"http://sites.google.com/site/r4statistics/mydataID.csv"; 
PROC IMPORT DATAFILE = myURL 
DBMS = CSV REPLACE 
OUT = mylib.mydata; 
GETNAMES = YES; 
DATAROW =2; 
RUN;
```

### **PYTHON:\(NEED TO CONFIRM !!!\)**

### **R:**

```
myURL <- "http:/sites.google.com/site/r4statistics/mydata.csv"
mydata<- read.csv(myURL)
```

## 4. Reading text from the clipboard

### SAS: \(NEED TO CONFIRM\)

### PYTHON:

read\_clipboard: version of read\_table that reads data from the clipboard. useful for converting tables from web pages

## R：

#### ** a. Copy a column of numbers or words, then :**

```
 myvector <- readClipboard()
```

#### ** b. Open mydata.csv, select & copy contents, then :**

```
 mydata <- read.delim ("clipboard", header = TURE)
```

#### ** c. Missing values for character varables**

```
 mydata <- read.csv("mydataID.csv", row.names = "id", strip.white = TRUE, na.strings = "")
```

#### **d. Skipping variables in delimited text files**

`myCols <- read.delim("mydata.tab", strip.white = TRUE, na.strings = "", colClasses = c("integer", "integer", "character", "NULL", "NULL", "integer", "integer"))`

## 5. Reading text data within a program

### **R:**

#### **a. The stdin approach**

```
mydata <- read.csv(stdin())
 workshop, gender, q1, q2, q3, q4 
 1,1,f,1,1,5,1
```

#### **b. Blank line above ends input**

#### **c. The testConnection approach **

```
mystring <- "workshop, gender, q1, q2, q3, q4 
1, 1, f,1 1, 1, 5, 1" 
mydata <- read.csv(textConnection(mystring))
```

### **PYTHON:\(NEED TO CONFIRM!!!\)**

### **SAS:**

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

## 6. Reading multiple observations per line

### **R:**

#### **a. The stdin approach**

```
mylist <- scan(stdin(), 
what = list(id = 0, workshop = 0, gender = " ",
 q1 =0, q2 = 0, q3, = 0, q4 = 0)) 
 1 1 f 1 1 5 1
```

#### **b. Blank line above ends input**

```
mydata <- data.frame(mylist)
```

#### **c. The textConnection approach **

```
mystring <- "1 1 f 1 1 5 1"; 

mylist <- scan(textConnection(mystring), 

what = list(id =0, workshop = 0, gender = " ", 

q1 =0, q2 = 0, q3 = 0, q4 =0 )) 

mydata <- data.frame(mylist)
```

### **PYTHON:\(NEED TO CONFIRM\)**

### **SAS:**

```
DATA mydata; 
INPUT id workshop gender $q1 - $q4 @@; 
DATALINES; 1 1 f 1 1 5 1 ; 
PROC PRINT; 
RUN;
```

## 7. Reading fixed-width text files : one record per case

### **R:**

#### **a. **

```
mydata <- read.fwf( 
file = "mydataFWF.txt",
 width = c(2, -1, 1, 1, 1,1,1 ), 
 col.names = c("id", "gender", "q1", "q2", "q3", "q4"), row.names = "id", 
 na.strings = "", 
 fill = TRUE, 
 strip.white = TRUE)
```

#### **b. Using "macro substitution".**

```
myfile <- "mydataFWF.txt" 
myvariablenames <- c("id", "gender", "q1", "q2", "q3", "q4")
 myvariablewidths <- c(2, -1, 1,1,1,1,1) 
 mydata <- read.fwf( file = myfile, width = myVariableWidths, col.names = myVariableNames, 
 row.names = "id", na.strings = " ", fill = TRUE, strip.white = TRUE)
```

### **PYTHON: \(NEED TO CONFIRM\)**

### **SAS:**

```
LIBNAME mylib 'C:\myRolder'; 
DATA myLib.mydata; 
INFILE '\myRfolder\mydataFWF.txt' 
MISSOVER; 
INPUT ID 1-2 WORKSHOP 3 GENDER $ 4 q1 5 q2 6 q3 7 q4 8; 
RUN;
```

## 8. Reading fixed - width text files, two or more records per case

### **R:**

```
myfile <- "mydataFWF.txt" 
myVariableNames <- c ("id", "workshop", "gender", "q1", "q2", "q3", "q4", "q5", "q6", "q7", "q8") 
myRecord1Widths <- c(2,1,1,1,1,1,1) 
myRecord2Widths <- c（-2， -1， -1， 1，1，1，1） 
myVariableWidths <- list(myRecord1Widths, myRecord2Widths) 
mydata <- read.fwf( file = myfile, width = myVariableWidths, col.names = myVariableWidths, row.names = "id", na.strings = " ", fill = TRUE, strip.white = TRUE)
```

### **PYTHON:\(NEED TO CONFIRM\)**

### **SAS:**

```
DATA temp; 
INFILE '\myRfolder\mydataFWF.txt' MISSOVER; 
INPUT \#1 id 1-2 workshop 3 gender 4 q1 5 q2 6 q3 7 q4 8 \#2 q5 5 q6 6 q7 7 q8 8; 
PROC PRINT; 
RUN;
```

## 9. Reading excel files

### **R: **

```
library("xlsReadWrite") 
xls.getshlib():
```

can get a binary file that is not distributed through CRAN

```
mydata <- read.xls("mydata.xls")
```

### **PYTHON:**

pd.read\__excel\('foo.xlsx', 'Sheet1', index\_col = None, na\_values = \['NA'\]\)_

### **SAS:**

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

## 10. Reading from relational databases

### R:

```
library("RODBC") 
myConnection <- odbcConnectExcel("mydata.xls") 
mydata <- sqlFetch(myConnection, "Sheet1") 
close(myConnection)
```

### PYTHON:

### SAS:

## 11. Reading HDF5\(only for PYTHON\)

## 11. Reading data from SAS \(only for R\)

```
library("foreign")   
mydata <- read.ssd("c:/myRfolder", "mydata",   
sascmd - "C:/Program files/SAS/SASFoundation/9.2/sas.exe")                                                                                                  
library("Hmisc")                                                                                                                                            
mydata <- sasxport.get("mydata.xpt")
```

## 12. Write data from SAS and read it into R\(only for R\)

```
LIBNAME mylib 'C:\myRfolder'; 
LIBNAME To\_R xport '\myRfolder\mydata.xpt'; 
DATA To_R.mydata; 
set mylib.mydata; 
RUN; 

\# read a SAS data set 
\# read ssd or sas7bdat if you have SAS installed

library("foreign") 

mydata <- read.ssd("c:/myRfolder“， ”mydata", sascmd = "C:/program files /SAS/SASFoundation/9.2/sas.exe") 

\# reads SAS export format without installing SAS library("foreign") 
library("Hmisc") 
mydata <- sasxport.get("mydata.xpt")
```

## 13. Writing delimited text files

### **SAS: **

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

### **PYTHON:**

```
data.to_csv('ch06/out.csv')
or
data.to_csv(sys.stdout,sep = '|')
```

demote the missing value by some other sentinel value;

```
data.to_csv(sys.stdout, na_rep = 'NULL')
```

```
or 

data.to_csv(sys.stdout,index = False, header = False)

or

dat.to_csv(sys.dout,index = False, cols = ['a', 'b', 'c'])
```

### **R:**

```
1. write.csv(mydata, "mydataFromR.csv") 
2. write.table(mydata, "mydataFromR.txt") 
3. write.table(mydata, file = "mydataFromR.txt", 
quote = FALSE, sep = "\t", na = " ", 
row.names = TRUE, col.names = TRUE)
```

## 14. Viewing a text fileViewing a text file\(only for R\)

```
file.show("mydataFromR.csv")
```

## 15. Writing Excel files

### **R:**

```
library("xlsReadWrite")
xls.getshlib() 
load("mydata.RData") 
write.xls (mydata, "mydataFromR.xls")
```

### **SAS: **

```
LIBNAME mylib "c:\myFolder"; 
PROC EXPORT DATA = mylib.mydata 
OUTFILE = "C:\myFolder\mydata.xls" 
DBMS = EXCELCS LABEL REPLACE; 
SHEET = "mydata"; 
RUN;
```

### **PYTHON: **

df.to\__excel \('foo.xlsx', sheet\_name = 'Sheet1'\)_

## 16. Writing to relational databases

### R:

```
library("RODBC") 
myConnection <- odbcConnectExcel("mydataFromR.xls", readOnly = FALSE) 
sqlSave(myConnection, mydata) 
close(myConnection)
```

### PYTHON:

### SAS:

## 

## 17. Writing data to SAS \(only for R?\)

```
library("foreign") 
write.foreign (mydata, datafile = "mydataFromR.csv", codefile = "mydata.sas", package = "SAS")
```

## 18. library



