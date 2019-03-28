---
title: "STA 4320 Homework 8"
author: "John Doe"
date: "12/5/18"
output: 
  html_document:
    keep_md: true
---

# Question 6 ISLR 5.4.5


## Solution.

### Part (b)

#### i) 

```r
set.seed(37128)

library(ISLR)
```

```
## Warning: package 'ISLR' was built under R version 3.4.4
```

```r
# proprtion divided into training and test sets
fractionTraining <- 0.5
fractionTesting <- 0.5

# gather sample size for training and test sets
nTraining <- floor(fractionTraining*nrow(Default))
nTest <- floor(fractionTesting*nrow(Default))

# find indices for training and test sets
indicesTraining <- sort(sample(1:nrow(Default),size=nTraining))
indicesTesting <- setdiff(1:nrow(Default), indicesTraining)

# FINISH...

train.set <- Default[indicesTraining, ]
test.set <- Default[indicesTesting, ]

# Making sure of no overlap:
dim(train.set)
```

```
## [1] 5000    4
```

```r
dim(test.set)
```

```
## [1] 5000    4
```

```r
intersect(train.set, test.set)
```

```
## data frame with 0 columns and 0 rows
```

```r
dim(setdiff(test.set, train.set)) # Should be same size as the sets
```

```
## [1] 5000    4
```

#### ii)

```r
fit <- glm(default ~ income + balance, data=train.set, family="binomial")
summary(fit)
```

```
## 
## Call:
## glm(formula = default ~ income + balance, family = "binomial", 
##     data = train.set)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.0832  -0.1462  -0.0617  -0.0236   3.6789  
## 
## Coefficients:
##               Estimate Std. Error z value Pr(>|z|)    
## (Intercept) -1.121e+01  6.026e-01 -18.603  < 2e-16 ***
## income       2.182e-05  7.074e-06   3.085  0.00203 ** 
## balance      5.370e-03  3.124e-04  17.191  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 1388.9  on 4999  degrees of freedom
## Residual deviance:  781.1  on 4997  degrees of freedom
## AIC: 787.1
## 
## Number of Fisher Scoring iterations: 8
```

#### iii)

```r
predictions <- rep("No", nTraining)
px <- predict(fit, test.set, type="response")
predictions[px > 0.5] <- "Yes"

table(test.set$default, predictions) # Checking predictions vs actual
```

```
##      predictions
##         No  Yes
##   No  4809   14
##   Yes  126   51
```

#### iv)

```r
1-mean(test.set$default == predictions) # Subtract from 1 to get % wrong.
```

```
## [1] 0.028
```

### Part (d)

```r
fit <- glm(default ~ income + balance + student, data=train.set, family="binomial")

summary(fit)
```

```
## 
## Call:
## glm(formula = default ~ income + balance + student, family = "binomial", 
##     data = train.set)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -2.0098  -0.1464  -0.0604  -0.0232   3.6383  
## 
## Coefficients:
##               Estimate Std. Error z value Pr(>|z|)    
## (Intercept) -1.064e+01  6.950e-01 -15.308   <2e-16 ***
## income       7.176e-06  1.175e-05   0.610    0.542    
## balance      5.432e-03  3.164e-04  17.165   <2e-16 ***
## studentYes  -5.326e-01  3.403e-01  -1.565    0.118    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 1388.89  on 4999  degrees of freedom
## Residual deviance:  778.67  on 4996  degrees of freedom
## AIC: 786.67
## 
## Number of Fisher Scoring iterations: 8
```

```r
predictions <- rep("No", nTraining)
px <- predict(fit, test.set, type="response")
predictions[px > 0.5] <- "Yes"

table(test.set$default, predictions) # Checking predictions vs actual again
```

```
##      predictions
##         No  Yes
##   No  4811   12
##   Yes  126   51
```

```r
1-mean(test.set$default == predictions)
```

```
## [1] 0.0276
```

The reduction of error rate with the dummy variable(0.028 to 0.0276) is extremely minor, so I would say there isn't really a difference in including the dummy variable for *student* or not.

# Question 7

### Part (a) 


```r
set.seed(37128)

library(ISLR)
library(boot) # cv
library(MASS) # lda/qda

# proprtion divided into training and test sets
fractionTraining <- 0.5
fractionTesting <- 0.5

# gather sample size for training and test sets
nTraining <- floor(fractionTraining*nrow(Default))
nTest <- floor(fractionTesting*nrow(Default))

# find indices for training and test sets
indicesTraining <- sort(sample(1:nrow(Default),size=nTraining))
indicesTesting <- setdiff(1:nrow(Default), indicesTraining)

# FINISH...

train.set <- Default[indicesTraining, ]
test.set <- Default[indicesTesting, ]

fit.lda <- lda(default ~ income + balance, data=train.set, family="binomial")

fit.qda <- qda(default ~ income + balance, data=train.set, family="binomial")

#lda
predictions <- rep("No", nTraining)
px <- predict(fit.lda, test.set, type="response")
predictions[px$posterior[,2] > 0.5] <- "Yes"
1-mean(test.set$default == predictions)
```

```
## [1] 0.0294
```

```r
table(test.set$default, predictions)
```

```
##      predictions
##         No  Yes
##   No  4815    8
##   Yes  139   38
```

```r
#qda
predictions <- rep("No", nTraining) # resetting predictions
px <- predict(fit.qda, test.set, type="response")
predictions[px$posterior[,2] > 0.5] <- "Yes"
1-mean(test.set$default == predictions)
```

```
## [1] 0.0284
```

```r
table(test.set$default, predictions)
```

```
##      predictions
##         No  Yes
##   No  4815    8
##   Yes  134   43
```

### Part (b)

```r
fit <- glm(default ~ ., data=Default, family="binomial")

sum.err <- c(0)
for(i in 1:20){
  sum.err[i] <- cv.glm(Default, fit, K=10)$delta[1]
}
sum.err
```

```
##  [1] 0.02141375 0.02143738 0.02141551 0.02135867 0.02140263 0.02141675
##  [7] 0.02136222 0.02138259 0.02138410 0.02143942 0.02138861 0.02145813
## [13] 0.02139237 0.02143612 0.02138652 0.02135633 0.02141411 0.02139125
## [19] 0.02138953 0.02139828
```

```r
cat("Mean of the errors: ", mean(sum.err))
```

```
## Mean of the errors:  0.02140121
```

### Part (c)
I ran the 10-fold CV 20 times and all of them were around 0.0214, while our other prediction errors were around 0.028. This is only less than 1% to put things into perspective, but the error for 10-fold CV does seem to be lower regularly.

### Part (d)
The Logistic Regression, LDA, and QDA errors were all extremely close. LDA had the highest error at 0.0294, but they all seemed to be around the 0.028 range. 
