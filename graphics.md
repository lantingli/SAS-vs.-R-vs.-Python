
\\ # get rid of missing values for facets
mydata100 <- na.omit(mydata100)
library("ggplot2")

\# 1. Bar plot
    \\# bar plot -vertical
      qplot(workshop, geom = "bar")
      ggplot(mydata100, aes(workshop)) + geom_bar()
      
    \\# bar plot-- horizontal
      qplot(workshop, geom = "bar") + coord_flip()
      ggplot(myata100, aes(workshop)) + geom_bar() + coord_flip()
      
     \\# bar plot-single bar stacked
       qplot(factor(""), fill = workshop, geom = "bar", xlab = "") + scale_fill_grey(start = 0, end = 1)
       
       ggplot(mydata100,
         ase(factor(""), fill = workshop)) +
         geom_bar() +
         scale_x_discrete("") +
         scale_fill_grey(start = 0, end = 1)

\# 2. Pie plot
   \\# pit charts, same as stacked bar but polar coordinates
     qplot(factor(""), fill = wokshop, geom = "bar", xlab = "") +
     coor_polar(theta = "y") +
     scale_fill_grey(start = 0, end = 1)
     ggplot(mydata100, 
       aes(factor(""), fill = workshop)) +
         geom_bar(width = 1) +
         scale_x_discrete("") +
         coord_polar(theta = "y") +
         scale_fill_grey (start = 0, end = 1)
\# 3. Bar plots for groups
\# 4. plots by group or level
\# 5. presummarized data
\# 6. dot charts
\# 7. adding titles and labels
\# 8. Histograms and density plots
\# 9. Normal QQ plots
\# 10. strip plots
\# 11. scatter plots and line plots
\# 12. box plots
\# 13. error bar plots
\# 14. geographic maps
\# 15. Logarithmic axes
\# 16. multiple plots on a page
\# 17. saving ggplot2 graphs to a file
\# 18. summary of graphics elements and parameters
