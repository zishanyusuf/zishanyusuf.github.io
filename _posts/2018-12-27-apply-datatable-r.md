---
layout: post
title: "Using Apply functions in data.table in R"
date: 2018-12-25
comments: true
share: true
---

* Table of Contents
{:toc}

# 1. USING FUNCTIONS IN DATA.TABLE OPERATIONS 
---

Well... Finally some placeholder to write my own Blog about data.table in R that i will continue adding as i learn. In general based on my studies and experiments, it's known that data manipulation with data.table is [faster][1] than python pandas. Hence, i am going to put together my few explorations with data.table. 

## 1.1 COLUMNs Wise Operations in data.table 

Consider the following example:

```{r}
#Define a data.table for sake of experiment
DT <- data.table(V1=c(1L,2L), V2=LETTERS[1:3],V3=round(rnorm(4),4),V4=1:12)
#Perform summation as given in data.table manual/literature
DT[, lapply(.SD, sum), by=V2]
```

This is a data manipulation that was carried over columns with grouping on a particular column (V2). Well that was straightforward.
What about specifying some user defined function while doing COLUMNS wise operations in data.table. Well...let's do it.

```{r}
DT[, lapply(.SD, function(x) {
     sum(x) + 2*x[1]
 }), by=V2]
```

What did we do? We summed up each column and we again added twice value of first element in each column. Well go on, run the code and see the results for yourself.

## 1.2 ROWs Wise Operations in data.table 
I wondered can i do the similar operations over ROW values instead of columns. Well, yes, you guessed it right - use "apply".

```{r}
#Re-define the same data.table
DT <- data.table(V1=c(1L,2L), V2=LETTERS[1:3],V3=round(rnorm(4),4),V4=1:12)

DT[, apply(.SD, 1, sum), by=V2]
#OR,
DT[, V5 := apply(.SD, 1, sum), by=V2] # ":=" Adds back the summation value to DT table
```

Have you run started running the code yet? Come on, run it and see it's results. Because things are getting harder. Next, i will specify user defined function for ROWs operations

```{r}
DT[, apply(.SD, 1, function(x){
  x[1]+x[3]*5
}), by=V2]

#OR,
DT[, V6 := apply(.SD, 1, function(x){ # ":=" Adds back the summation value to DT table
  x[1]+x[3]*5
}), by=V2]

```

Trying to figure out what function i wrote? Hmm...so you have not run the code yet! I know you will figure it out.

Let's move on. 
Powered by [Jekyll](http://jekyllrb.com) and tutored by Hank Quinlan, Thank You!! It actually is a lot easier than I thought it was going to be.


[1]: https://github.com/Rdatatable/data.table/wiki/Benchmarks-%3A-Grouping "Data Manipulation with Python Pandas and R Data.Table"
