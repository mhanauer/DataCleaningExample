# DataCleaningExample
---
title: "DataCleaning"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Here is an example of how to clean data from a survey using data from a local school district in Indiana.  First we set the working directory to where the data is stored and then load the data using read.csv function.  
```{r}
setwd("~/Desktop/QualData")
district = read.csv("MCCSCStaffSurvey.csv", header = TRUE)
```
First, we visually inspected the data to see if Qualtrics, the survey program that we used, provided any additional data that we did not need for the analysis. When we exported the data from qualtrics, the first two rows were additional information that qualtrics provides that is not relevant to the study.  Therefore, we deleted the first two rows across all the variables.
```{r}
district = district[-c(1:2), ]
```
Next we selected the variables from the dataset that we were interested in analyzing.  In this case, we were interested in evaluating the differences between the perceptions of current professional development for Social and Emotional learning of staff who are exclusively primary and secondary teachers.  Therefore, we selected Q44, which is a categorical variable identifying the type of staff (e.g. primary, secondary, principal).  Then we selected Q1_6 which provided Likert Scale responses to pro.  

```{r}
district2 = district[c("Q1_6")]
district3 = district[c("Q44")]
```
Because the responses to their perceptions of professional development are in a Likert Scale format, we need to transform each of the responses into a numerical value so we can conduct data analysis with them.  We transformed the data using the apply function.  

The apply function can be used as a more compact form of an if else statement that allows us to transform the categorical responses (e.g. Strongly agree) into numerical ones.  We use three arguments on the apply function here.  First is the data, which is district2.  Second is how we want the apply to review the data.  2 means we want R to review the data going from column to column or variable by variable.  Finally, we created a function with an ifelse statement that says change a categorical value (e.g. Strongly agree) to a numerical value and leave the rest of the data points alone.  We then need to transform that back into a data frame after we apply the apply function so that we can change the other categorical responses into numbers.  Although, there is likely a more efficient way for transforming the data, the strategy presented below does work and can be more intuitive than a large for loop trying to make all of these changes at once. 
```{r}
district2 = apply(district2, 2, function(x){ifelse(x == "Strongly agree", 7, x)})
district2 = as.data.frame(district2)
district2 = apply(district2, 2, function(x){ifelse(x == "Agree", 6, x)})
district2 = as.data.frame(district2)
district2 = apply(district2, 2, function(x){ifelse(x == "Somewhat agree", 5, x)})
district2 = as.data.frame(district2)
district2 = apply(district2, 2, function(x){ifelse(x == "Neither agree nor disagree", 4, x)})
district2 = as.data.frame(district2)
district2 = apply(district2, 2, function(x){ifelse(x == "Somewhat disagree", 3, x)})
district2 = as.data.frame(district2)
district2 = apply(district2, 2, function(x){ifelse(x == "Disagree", 2, x)})
district2 = as.data.frame(district2)
district2 = apply(district2, 2, function(x){ifelse(x == "Strongly disagree", 1, x)})
district2 = as.data.frame(district2)
```
Next we create numerical indexes for the Primary school teachers (1) and Secondary school teachers (2) so that we can select Likert scale responses for those two groups of staff.  Again we use the apply function.

```{r}
district3 = apply(district3, 1, function(x){ifelse(x == "Primary school teacher", 1, x)})
district3 = as.data.frame(district3)
district3 = apply(district3, 1, function(x){ifelse(x == "Secondary school teacher", 2, x)})
district3 = as.data.frame(district3)
```
Now that we have data coded for each of the variables of interest, we can combine them back into one data frame using the cbind function.
```{r}
district4 = cbind(district2, district3)
```
Then we grab data where a respondent is either a primary (1) or secondary teacher (2) exclusively.  Because those staff members are coded as 1 or 2, we can grab data for those two groups using the subsetting logic displayed below. Finally, we rename the variables to be more meaningful, SELPDScore to represent their Likert scale response for their satisfaction of SEL professional development and TeacherType, which distinguishes between primary (1) and secondary (2) teachers.
```{r}
district5 = district4[district4$district3 %in% c(1,2), ]
names(district5) = c("SELPDScore", "TeacherType")
head(district5)
```
Finally, we create a csv file that can be used in data analysis.
```{r}
write.csv(district5, "district5.csv")
```
