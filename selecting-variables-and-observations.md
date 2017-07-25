# R

1. The subset function

Summary \(

Subset\(mydata,

Subset = gender = “m”,

Select = c\(workshop, q1: q4\)\)

\)

Summary \(

Subset \(mydata, gender == “m”,

C\(workshop, q1:q4\)\)

\)

1. With logical selections and variable names

Summary \(

Mydata \[which \(gender ==”m”\),

c\(“workshop”, “q1”, “q2”, “q3”, “q4”\)\]

\)

myMales &lt;- which \(gender == “m”\)

myVars &lt;- c\(“workshop”, “q1”, “q2”, “q3”, “q4”\)

myVars &lt;- c\(“workshop”, paste\(q, 1:4, sep = “ “\)\)

summary \(mydata\[myMales, myVars\]\)

1. Using names to select both observations and variables
2. Using numeric index values to select both observations and variables
3. Using logic to select both observations and variables

# PYTHON



