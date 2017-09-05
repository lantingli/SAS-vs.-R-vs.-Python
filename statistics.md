\# 1. descriptive statistics

  SAS: 
  
 

```
 LIBNAME mylib 'C:\myRfolder';
   data temp;
     set mylib.mydata100;
     mydiff = q2 -q1;
    run;
```


    
   \# basic stats in compact form;
  

```
 proc means;
     var q1-- posttest;
   run;
```


   
   \# basic stats of every sort;
   

```
 proc univariate;
      var q1 -- posttest;
    run;
```


    
  \# frequencies & percents;
   

```
  proc freq;
       tables workshop--q4;
       run;
```


  \# chi-square
  

```
  proc freq;
      tables workshop *gender/chisq;
      run;
```


      
  \# measures of associations;
   

```
  \\# pearson correlations;
    proc corr;
      var q1 -q4;
      run;
      
    \\# spearman correlations;
   proc corr spearman;
     var q1 -q4;
   run;

```

   
   \# linear regression;
  

```
 proc reg;
     model q4 = q1 - q3;
   run;
```


   
   \# group comparisons
     

```
 \\# independent samples t-test
      proc ttest;
        class gender;
        var q1;
        run;
        
 \\# nonparametric version of above
          using Wilcoxon/Mann-WHitney test;
      proc npar1way;
        class gender;
        var q1;
        run;
        
  \\# paried samples t-test;
        proc ttest;
          paried pretest * posttest;
        run;
        
  \\# nonparametric version of above using
          both signed rank test and sign test
        proc univariate;
          var mydiff;
          run;
```


          
\# oneway analysis of variance(ANONVA)
    

```
proc glm;
      class workshop;
      model posttest = workshop;
      means workshop/tukey;
    run;
```


    
\# nonparametric version of above using 
     Kruskal-wallis test
  

```
   proc npar1way;
       class workshop;
       var posttest;
     run;
```


     
     
      
      
    
  
  
  PYTHON:
  
  
  
  R:
  
  \# frequencies & univariate statistics
  
 \\# deducer's frequencies() function
        

```
 library("Hmisc")
         describe(mydata100)
```


 \\# R's build-in function
     

```
  summary(mydata100)

```

 \\# the flexible way using build-in functions
    

```
   table(workshop)
       table(gender)
```


 \\# proportions of valid values
      

```
       prop.table(table(workshop))
       prop.table(table(gender))
```


       
 \\# rounding off proportions
     
`round(prop.table(table(gender)), 2)`
       
 \\# converting proportions to percents
     
 `round(100* (prop.table(table(gender))))`
       
 \\# frequencies & univariate
 
    summary(mydata100)
       
 \\# means & std deviations
       

```
       options(width = 63)
       sapply(mydata100[3:8], mean, na.rm = TRUE)
       sapply(mydata100[3:8], sd, na.rm = TRUE)
```


             
\# 2. Cross-Tabulation

   \\# using the gmodels package
      

```
       library("gmodels")
       CrossTable(workshop, gender, chisq = TRUE, format = "SAS")
```


       
 \\# using build-in functions
 
   \# counts
        

```
         myWG <- table(workshop, gender)#crosstabulation format
         myWGdata<- as.data.frame(myWG) # summary or aggregation format
         chisq.test(myWG)
```


   \\# row proportions
   
       `prop.table(myWG, 1)`
         
   \\# column proportions
   
        `prop.table(myWG, 2)`
         
   \\# Total proportions
   
         `prop.table(myWG)`
         
   \\# Rounding off proportions
   
        `round(prop.table(myWG, 1), 2)`
        
   \\# row percents
   
        ` round(100 * prop.table(myWG, 1)))`
         
   \\# adding row and column totals
        

```
         addmargins(myWG, 1)
         addmargins(myWG, 2)
```


         
\# 3. Correlation

   \\# the rcorr.adjust function from the R commander package
       

```
        library("Rcmdr")
        load("mydata.RData")
        rcorr.adjust(mydata[3:6])
```


   \\# spearman correlations
   
       ` rcorr.adjust(mydata[3:6], type = "spearman")`
       
   \\# the built-in cor function
   
      ` cor(mydata[3:6], method = "pearson", use = "pairwise")`
      
   \\# the built -in cor.test function
   
       `cor.test(mydata$q1, mydata$q2, use = "pairwise")`
         
      
\# 4. Linear regression
  
 

```
 lm(q4 ~ q1 +q2+ q3, data = mydata100)
   mymodel <- lm(q4 ~ q1 +q2+ q3, data = mydata100)
   summary(mymodel)
   anova(mymodel) # same as summary result
```


   
   not complete!!! will add later!!!
   
\# 5. t -Test independent groups

   \\# independent samples t-test
      `t.test(q1 ~ gender, data = mydata100)`
      
  \\# same test; requires attached data;
       

```
       t.test(q1[gender =="Male"], 
              q1 [gender == "Female"])
```


  \\# same test using with() function
       

```
with(mydata100,
         t.test(q4[which(gender =="m")],
                q4[which(gender =="f")])
        )
```


  \\# same test using subset() function
     

```
 t.test(
      subset(mydata100, gender =="m", select = q4),
      subset(mydata100, gender == "f", select = q4)
      )
```


    
  \\# paired samples t-test
  
     ` t.test(posttest, pretest, paired = TRUE)`
  
        
\# 6. equality of variance 

    

```
      library("car")
      levene.test(posttest, gender)
      var.test(posttest ~ gender)
```


      
      
       
      
         
       
         
      
        
        
       
      
        
      
\# 7. paired or repeated measures
\# 8. wilcoxon-mann-whitney rank sum
\
\# Wilcoxon/Mann-Whitney test
wilcox.test(q1 ~ gender, data = mydata100)
\\# same test specified differently
wilcox.test(q1[gender =='Male‘]，
q1[gender =='Female'])
aggregate(q1, data.frame(gender),
median, na.rm = TRUE)

\# 9. Wilcoxon signed-rank test: paired groups

\\# wilcoxon signed rank test
wilcox.test(posttest, pretest, paired = TRUE)
median(pretest)
median(posttest)
\#10. sign test: paried groups
\
\# sign test
library("PASWR")
sign.test(posttest, pretest, conf.level = .95)\# 11. analysis of variance\\# analysis of variance (ANOVA)
aggregate(posttest, data.frame(workshop), mean, na.rm = TRUE)
library("car")
levene.test(posttest, workshop)
myModel <- aov(posttest ~ workshop, data = mydata100)
anova(myModel)
summary(myModel)
\\# type III sums of squares
library("car")
Anova(myModel, type = "III")
pairwise.t.test(posttest, workshop)

\# 13. the kruskal - wallis test
  \\3 nonparametric oneway ANOVA using kruskal.test(posttest~ workshop)
  pairwise.wilcox.test(posttest, workshop)
  aggregate(posttest, data.frame(workshop), median, na.rm = TRUE)