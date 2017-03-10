# DataCleaningExample
---
title: "DataCleaning"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

```{r}
setwd("~/Desktop")
mccsc = read.csv("MCCSCStaffSurvey.csv", header = TRUE); head(mccsc)
rbbcsc = read.csv("RBBCSCStaffSurvey.csv", header = TRUE); head(rbbcsc)
# Need to get rid of two rows at the top, because they are not part of the data.
# In the rows 27 to 37 there are two types of questions there probably just combine them.
mccsc = mccsc[c("Q1_1",	"Q1_2",	"Q1_3",	"Q1_4",	"Q1_5",	"Q1_6", "Q5_1",	"Q5_2",	"Q5_3",	"Q5_4",	"Q5_5",	"Q5_6", "Q6_1",	"Q6_8",	"Q6_5",	"Q6_6",	"Q6_7", "Q9_1",	"Q9_2",	"Q9_3",	"Q9_4",	"Q9_5",	"Q9_6", "Q32",	"Q34",	"Q36", "Q38",	"Q44")]; head(mccsc)
rbbcsc = rbbcsc[c("Q1_1",	"Q1_2",	"Q1_3",	"Q1_4",	"Q1_5",	"Q1_6", "Q4_1",	"Q4_2",	"Q4_3",	"Q4_4",	"Q4_5",	"Q4_6", "Q7_1",	"Q7_2",	"Q7_3",	"Q7_4",	"Q7_5",	"Q7_6", "Q11", "Q27",	"Q28", "Q10",	"Q12",		"Q13",	"Q14_1_1",	"Q14_15_1",	"Q14_2_1",	"Q14_3_1",	"Q14_4_1",	"Q14_5_1",	"Q14_6_1",	"Q14_7_1",	"Q14_8_1",	"Q14_9_1",	"Q14_10_1",	"Q14_11_1",	"Q14_12_1",	"Q14_13_1",	"Q14_14_1",	"Q15")]; head(rbbcsc)

```
Next we delete the first two rows in the data set.
```{r}
mccsc = mccsc[-c(1:2), ]; head(mccsc)
rbbcsc = mccsc[-c(1:2), ]; head(mccsc)
```
Next select only the professional development questions for sel for each district and type of staff.  We create three different variables one with both variables of interest, one for seperate for each variable of interest.
```{r}
mccsc1 = mccsc[c("Q1_6", "Q44")]; head(mccsc)
mcccsc2 = mccsc[c("Q1_6")]
mccsc3 = mccsc[c("Q44")]
```
Next figure out how to tranform the likert scale into 1:7 numbers
```{r}
mccsc2 = apply(mccsc, 1, function(x){ifelse(x == "Strongly agree", 7, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2)
mccsc2 = apply(mccsc2, 1, function(x){ifelse(x == "Agree", 6, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2)
mccsc2 = apply(mccsc2, 1, function(x){ifelse(x == "Somewhat agree", 5, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2)
mccsc2 = apply(mccsc2, 1, function(x){ifelse(x == "Neither agree nor disagree", 4, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2)
mccsc2 = apply(mccsc2, 1, function(x){ifelse(x == "Somewhat disagree", 3, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2)
mccsc2 = apply(mccsc2, 1, function(x){ifelse(x == "Disagree", 2, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2)
mccsc2 = apply(mccsc2, 1, function(x){ifelse(x == "Strongly disagree", 1, x)}); mccsc2
mccsc2 = as.data.frame(mccsc2); mccsc2
mccsc2 = as.data.frame(mccsc); mccsc2
```
Next put the variables for the PD question and the type of staff back to together
```{r}
mccsc2 = cbind(mccsc, mccsc1); mccsc
```
Next we create three levels for the types of professional staff.  We create three levels  Primary school teacher, Secondary school teacher, and other.  What we have here are people who sole job is a primary or secondary school teacher

```{r}
mccsc3 = apply(mccsc3, 1, function(x){ifelse(x == "Primary school teacher", 1, x)}); mccsc3
mccsc3 = as.data.frame(mccsc3); mccsc3
mccsc3 = apply(mccsc3, 1, function(x){ifelse(x == "Secondary school teacher", 2, x)}); mccsc3
mccsc3 = as.data.frame(mccsc3); mccsc3
```
Next combine the results



