| SAS | 1. Reading delimited text files:                                                   `proc import out = mylib.mydata                       datafile = "c"\myRfolder\mydataID.csv"              DBMS = CSV    REPLACE;                              GETNAMES = YES;                                    DATAROW = 2;                                        RUN;`                                                                                          `PROC PRINT; RUN;`                                                                2. Tab delimited files:                                                                  `PROC IMPORT OUT = mylib.mydata                     DATAFILE = "C:\myRworkshop\mydataID.tab"            DBMS = TAB  REPLACE;                                 GETNAMES = YES;                                     DATAROW =2;                                         RUN;`                                                                                       3. Reading from a web site:                                                     `FILENAME myURL URL "`[`http://sites.google.com/site/r4statistics/mydataID.csv`](http://sites.google.com/site/r4statistics/mydataID.csv)`";                                                                            PROC IMPORT DATAFILE = myURL                        DBMS = CSV  REPLACE                                 OUT = mylib.mydata;                                 GETNAMES = YES;                                     DATAROW =2;                                         RUN;` |
| :--- | :--- |
|  | reading text data within a program |
| R | 1. the stdin approach                                                                         mydata &lt;- read.csv\(stdin\(\)\)                                                      workshop, gender, q1, q2, q3, q4                                                 1,1,f,1,1,5,1                                                                                2. blank line above ends input                                                    3. the testConnection approach                                                  mystring &lt;- "workshop, gender, q1, q2, q3, q4                                                 1, 1, f,1 1, 1, 5, 1"                                                     mydata &lt;- read.cs\(textConnection\(mystring\)\) |
| Python |  |
| SAS | LIBNAME myLib 'C:\myRfolder';                                                         DATA mylib.mydata;                                                                     INFILE DATALINES DELIMITER = ','                                               MISSOVER DSD firstobs = 2;                                                    INPUT id workshop gender $ q1 q2 q3 q4;                               DATALINES;                                                                                   id, workshop, gender, q1 q2, q3, q4                                            1,1,f,1,1,5,1                                                                                 PROC PRINT; RUN; |
|  | Reading multiple observations per line |
| R | 1. mylist &lt;- scan\(stdin\(\), what = list\(id = 0, workshop = 0, gender = " ", q1 =0, q2 = 0, q3, = 0, q4 = 0\)\)                                         1 1 f 1 1 5 1                                                                               2. Blank line above ends input                                                    mydata &lt;- data.frame\(mylist\)                                                      3. the textConnection approach                                                 mystring &lt;- "1 1 f 1 1 5 1";                                                            mylist &lt;- scan\(textConnection\(mystring\), what = list\(id =0, workshop = 0, gender = " ", q1 =0, q2 = 0, q3 = 0, q4 =0 \)\)       mydata &lt;- data.frame\(mylist\) |
| Python |  |
| SAS | DATA mydata;                                                                                INPUT id workshop gender $q1 - $q4 @@;                                DATALINES;                                                                                   1 1 f 1 1 5 1 ;                                                                                  PROC PRINT; RUN; |
|  | Reading fixed-width text files : one record per case |
| R | 1. mydata &lt;- read.fwf\(                                                                         file = "mydataFWF.txt",                                                                 width = c\(2, -1, 1, 1, 1,1,1 \),                                                          col.names = c\("id", "gender", "q1", "q2", "q3", "q4"\),                    row.names = "id",                                                                          na.strings = "",                                                                               fill = TRUE,                                                                                     strip.white = TRUE\)                                                                  2. myfile &lt;- "mydataFWF.txt"                                                            myvariablenames &lt;- c\("id", "gender", "q1", "q2", "q3", "q4"\)       myvariablewidths &lt;- c\(2, -1, 1,1,1,1,1\)                                        mydata &lt;- read.fwf\(                                                                      file = myfile,                                                                                   width = myVariableWidths,                                                          col.names = myVariableNames,                                                 row.names = "id",                                                                          na.strings = " ",                                                                              fill = TRUE,                                                                                     strip.white = TRUE\) |
| Python |  |
| SAS | LIBNAME mylib 'C:\myRolder';                                                     DATA myLib.mydata;                                                                    INFILE '\myRfolder\mydataFWF.txt' MISSOVER;                       INPUT ID 1-2 WORKSHOP 3 GENDER $ 4 q1 5 q2 6 q3 7 q4 8;  RUN; |
|  | Reading fixed - width text files, two or more records per case |
| R | myfile &lt;- "mydataFWF.txt"                                                             myVariableNames &lt;- c \("id", "workshop", "gender", "q1", "q2", "q3", "q4", "q5", "q6", "q7", "q8"\)                                                    myRecord1Widths &lt;- c\(2,1,1,1,1,1,1\)                                            myRecord2Widths &lt;- c（-2， -1， -1， 1，1，1，1）            myVariableWidths &lt;- list\(myRecord1Widths, myRecord2Widths\)                                                                                                 PLUG IN:                                                                                        mydata &lt;- read.fwf\(                                                                        file = myfile,                                                                                   width = myVariableWidths,                                                          col.names = myVariableWidths,                                                 row.names = "id",                                                                          na.strings = " ",                                                                              fill = TRUE,                                                                                     strip.white = TRUE\) |
| Python |  |
| SAS | DATA temp;                                                                                    INFILE  '\myRfolder\mydataFWF.txt' MISSOVER;                       INPUT                                                                                             \#1 id 1-2 workshop 3 gender 4 q1 5 q2 6 q3 7 q4 8                 \#2                                                 q5 5 q6 6 q7 7 q8 8;                 PROC PRINT;                                                                               RUN; |
|  | Reading excel files |
| R | library\("xlsReadWrite"\)                                                                   xls.getshlib\(\): can get a binary file that is not distributed through CRAN                                                                                     mydata &lt;- read.xls\("mydata.xls"\) |
| Python |  |
| SAS | LIBNAME mylib "c:\myRfolder";                                                   PROC IMPORT OUT = mylib.mydata                                              DATAFILE = "C:\myRfolder\mydata.xls"                                     DBMS = EXCELCS REPLACE;                                                       RANGE = "Sheet1$";                                                                     SCANTEXT = YES;                                                                         USEDATE = YES;                                                                           SCANTIME = YES; |
|  | Reading from relational databases |
| R | library\("RODBC"\)                                                                            myConnection &lt;- odbcConnectExcel\("mydata.xls"\)                  mydata &lt;- sqlFetch\(myConnection, "Sheet1"\)                           close\(myConnection\) |
| Python |  |
| SAS |  |
|  | Reading data from SAS |
| R | library\("foreign"\)                                                                            mydata &lt;- read.ssd\("c:/myRfolder", "mydata",                               sascmd - "C:/Program files/SAS/SASFoundation/9.2/sas.exe"\)                                                                                                  library\("Hmisc"\)                                                                            mydata &lt;- sasxport.get\("mydata.xpt"\) |
|  |  |
| Python |  |
| SAS |  |
|  | write data from SAS and read it into R |
| R | LIBNAME mylib 'C:\myRfolder';                                                    LIBNAME To\_R xport '\myRfolder\mydata.xpt';                           DATA To\_R.mydata;                                                                        set mylib.mydata;                                                                      RUN;                                                                                               \#\# read a SAS data set                                                                \# read ssd or sas7bdat if you have SAS installed                     library\("foreign"\)                                                                           mydata &lt;- read.ssd\("c:/myRfolder“， ”mydata",                            sascmd = "C:/program files /SAS/SASFoundation/9.2/sas.exe"\)                                                                                             \# reads SAS export format without installing SAS                    library\("foreign"\)                                                                           library\("Hmisc"\)                                                                            mydata &lt;- sasxport.get\("mydata.xpt"\) |
|  | Writing delimited text files |
| SAS | PROC PRINT DATA = mylib.mydata; run;                                     PROC EXPROT DATA = mylib.mydata                                                  outfile = "C"\myFolder\mydataFromSAS.csv"                           DBMS = CSV REPLACE;                                                               PUTNAMES = YES;                                                              RUN;                                                                                               PROC EXPORT DATA = mylib.mydata                                                 outfile = "C"\myFolder\mydataFromSAS.txt"                             DBMS = TAB REPLACE;                                                               PUTNAMES = YES;                                                                 RUN; |
| Python |  |
| R | 1. write.csv\(mydata, "mydataFromR.csv"\)                                  2. write.table\(mydata, "mydataFromR.txt"\)                                3. write.table\(mydata,                                                                          file = "mydataFromR.txt",                                                              quote = FALSE,                                                                              sep = "\t",                                                                                      na = " ",                                                                                           row.names = TRUE,                                                                      col.names = TRUE\) |
|  | Viewing a text file |
| R | file.show\("mydataFromR.csv"\) |
| Python |  |
| SAS |  |
|  | Writing Excel files |
| R | library\("xlsReadWrite"\)                                                                   xls.getshlib\(\)                                                                                 load\("mydata.RData"\)                                                                  write.xls \(mydata, "mydataFromR.xls"\) |
| Python |  |
| SAS | LIBNAME mylib "c:\myFolder";                                                     PROC EXPORT DATA = mylib.mydata                                               OUTFILE = "C:\myFolder\mydata.xls"                                        DBMS = EXCELCS LABEL REPLACE;                                          SHEET = "mydata";                                                                  RUN; |
|  | Writing to relational databases |
| R | library\("RODBC"\)                                                                              myConnection &lt;- odbcConnectExcel\("mydataFromR.xls", readOnly = FALSE\)                                                                          sqlSave\(myConnection, mydata\)                                                 close\(myConnection\) |
| Python |  |
| SAS |  |
|  | Writing Data to SAS |
| R | library\("foreign"\)                                                                             write.foreign \(mydata,                                                                      datafile = "mydataFromR.csv",                                                    codefile = "mydata.sas",                                                              package = "SAS"\) |

3.1.1 Reading delimited text files  
1\) Reading comma-delimited text files:  
  Mydata &lt;- read.csv\(“mydata.csv”\)  
2\) Reading Tab-Delimited text files:  
  Mydata &lt;- read.delim\(“mydata.tab”\)  
1\) If you need to read a file that has multiple tabs or spaces between values, use the read.table function.  
2\) Reading text from a web site  
    myURL &lt;- http:\/\/sites.google.com\/site\/mydata.csv  
    mydata &lt;- read.csv\(myURL\)  
    myvector &lt;- readClipboard\(\)  
3\) Missing values for character variables  
    R will not set the blanks to missing unless you specifically tell it to do so.  
    In comma-delimited file, the missing value for gender was entered as a single space. Therefore, the argument na.char = “ “ added to any of the comma-delimited examples will set the value to missing. Note there is a single space between the quotes in that argument.  
    In tab-delimited file, the missing value for gender was entered as nothing between two tabs. Therefore, the argument na.char = “” added to any of the tab-delimited examples will set the value to missing, note that there is now no space between the quotes in that argument.  
    However, in both comma- and tab-delimited files, it is very easy to accidentally have blanks where you think there are none or to enter more than you meant to. Then your na.char setting will be wrong for some cases. It is best to use a solution that will get rid of all trailing blanks, that is what the argument strip.white = TRUE does.  
    For example:  
    Mydata &lt;- read.csv\(mydataID.csv”, row.names = “id”, strip.white = TRUE, na.string = “”\)  
        1. Reading text data within a program  
4\) The easy approach\(the sdtin approach\) : mydata &lt;- read.csv\(stdin\(\)\)  
        workshop, gender, q1, q2, q3,q4  
        1,1,f,1,1,15,1  
5\) blank line above ends input  
6\) the textconnection approach  
7\) that works when sourcing files  
        1. Reading multiple observations per line  
        Mylist &lt;- scan\(stdin\(\), what = list\(id = 0, workshop = 0, gender = “”,  
            Q1 = 0, q2 = 0, q3 = 0, q4 = 0\)\)  
3.1.4 Reading data from the keyboard  
3.1.5 Reading fixed-width text files, one record per case  
         Read.fwf  
3.1.6 Reading fixed-width text files, two or more records per case  
3.1.7 Reading excel files  
Xlsreadwrite package  
Mydata &lt;- read.xls \(“mydata.xls”\)  
3.1.8 Reading from relational databases  
Library（”RODBC”）  
Myconnection &lt;- odbcConnectExcel \(“mydata.xls”\)  
Mydata &lt;- sqlfetch\(myconnection,  “Sheet1”\)  
Close \(myconnection\)  
3.1.9 Reading data from SAS  
Library\(“foreign”\)  
Mydata &lt;- read.ssd\(c:\myfolder”, “mydata”, sascmd = “C:\/program files\/sas\/sas.exe”\)  
Xpt file:  
Library \(“foreign”\)  
Library\(“Himsc”\)  
Mydata &lt;- sasxport.get\(“mydata.xpt”\)  
3.1.10 Reading data from SPSS  
Mydata &lt;- read.spss\(“mydata.sav”, use.value.labels = TRUE, to.data.frame = TRUE\)  
Library\(“Himsc”\)  
Mydata &lt;- spss.get \(“mydata.por”\)  
3.1.11 Writing delimited text files  
3.1.12 Viewing a text file  
File.show\(“mydatafromR.csv”\)  
3.1.13 Writing excel files  
3.1.14 Writing to relational databases  
3.1.15 Writing data to SAS and SPSS

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



