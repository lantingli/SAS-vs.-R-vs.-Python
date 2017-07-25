# R

|  | R | PYTHON | SAS |
| --- | --- | --- | --- |
| **1. Reading Delimited Text files** |  |  |  |
| Reading Comma-delimited text files | Mydata &lt;- read.csv\(“mydata.csv”\) |  |  |
| _Reading Tab-delimited text files_ | Mydata &lt;- read.delim\(“mydata.tab”\) |  |  |
| _Reading text from a web site_ | myURL &lt;- http:\/\/sites.google.com\/site\/mydata.csv     mydata &lt;- read.csv\(myURL\) |  |  |
| _Reading text from the clipborad_ | myvector &lt;- readClipboard\(\) |  |  |
| _  Missing values for character variables_ |  |  |  |
| _Trouble with Tabs_ |  |  |  |
| _ Skipping variables in delimited text files_ |  |  |  |
| _Reading Character strings_ |  |  |  |
| **2. Reading Text data within a program** |  |  |  |
| _The easy approach_ |  |  |  |
| _ The more general approach_ |  |  |  |
| **3. Reading multiple observations per line** |  |  |  |
| **4. Reading data from the keyboard** |  |  |  |
| **5. Reading Fixed-Width text files, one record per case** |  |  |  |
| **6. Reading Fixed-Width text files, two or more records per case** |  |  |  |
| **7. Reading excel files** |  |  |  |
| **8. Reading from rational databases** |  |  |  |
| **9. Reading data from SAS** |  |  |  |
| **10.** **Reading data from SPSS** |  |  |  |
| **11.** **Writing delimited text files** |  |  |  |
| **12.** **Viewing a text file** |  |  |  |
| **13.** **Writing excel files** |  |  |  |
| **14. Writing to rational databases** |  |  |  |
| **15. Writing data to SAS and SPSS** |  |  |  |

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



