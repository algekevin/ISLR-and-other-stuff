---
title: "STA 4320 Homework 4"
author: "John Doe"
date: "10/3/18"
output: 
  html_document:
    keep_md: true
---

# Question 4 ISLR 3.7.10


## Solution.

### Part (a)


```r
library(ISLR)
```

```
## Warning: package 'ISLR' was built under R version 3.4.4
```


```r
carseats.lm <- lm(Sales ~ Price + Urban + US, data=Carseats)
summary(carseats.lm)
```

```
## 
## Call:
## lm(formula = Sales ~ Price + Urban + US, data = Carseats)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6.9206 -1.6220 -0.0564  1.5786  7.0581 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 13.043469   0.651012  20.036  < 2e-16 ***
## Price       -0.054459   0.005242 -10.389  < 2e-16 ***
## UrbanYes    -0.021916   0.271650  -0.081    0.936    
## USYes        1.200573   0.259042   4.635 4.86e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.472 on 396 degrees of freedom
## Multiple R-squared:  0.2393,	Adjusted R-squared:  0.2335 
## F-statistic: 41.52 on 3 and 396 DF,  p-value: < 2.2e-16
```

### Part (b)
#### Price
There *is* a linear relationship between **sales** and **price** as we can see from the low p-value. The estimate shows a slightly negative relationship though, meaning as prices increase, the sales will decrease. 

#### UrbanYes
UrbanYes indicates that the store is in an Urban location rather than rural. The extremely high p-value here suggests that there *isn't* a relationship between being in an urban location and the sales of child carseats. 

#### USYes
USYes indicates that the store is located in the US. The small p-value suggests that there *is* a relationship between being **located in the US** and **sales**. This relationship is positive as can be seen by the estimate, which means that if the store is located in the US the sales will be higher. 

### Part (c)
$$Sales = \beta_0 + \beta_1Price + \beta_2UrbanYes + \beta_3USYes$$
So filling everything in we get:

$$Sales = 13.043 - 0.054Price -0.022UrbanYes + 1.2USYes$$
Note that UrbanYes and USYes are only included in the case the store is in the locations specified.

### Part (d)
We can reject the null hypothesis $H_0: \beta = 0$ for both **price** and **USYes** as their p-values are small enough to indicate this.

### Part (e)

```r
carseats.lm2 <- lm(Sales ~ Price + US, data=Carseats)
summary(carseats.lm2)
```

```
## 
## Call:
## lm(formula = Sales ~ Price + US, data = Carseats)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6.9269 -1.6286 -0.0574  1.5766  7.0515 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 13.03079    0.63098  20.652  < 2e-16 ***
## Price       -0.05448    0.00523 -10.416  < 2e-16 ***
## USYes        1.19964    0.25846   4.641 4.71e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.469 on 397 degrees of freedom
## Multiple R-squared:  0.2393,	Adjusted R-squared:  0.2354 
## F-statistic: 62.43 on 2 and 397 DF,  p-value: < 2.2e-16
```

### Part (f)
Looking at the $R^2$ and the RSE we can see that both models from parts (a) and (e) fit the data very similarly. This should make sense because *Urban* showed to be statistically insignificant in part (a). The $R^2$ value in part (e) is very slightly higher, so it's a bit better of a fit for the data than the model from part (a) is.

### Part (g)

```r
confint(carseats.lm2)
```

```
##                   2.5 %      97.5 %
## (Intercept) 11.79032020 14.27126531
## Price       -0.06475984 -0.04419543
## USYes        0.69151957  1.70776632
```


# Question 5


## Solution.

### Part (a)
#### Forward model selection

I run linear regressions with just one predictor first to get the highest $R^2$ and I store that into the variable I call **r2** at the end, then repeat until we get to a set where no $R^2$ is better, then we stop.

One predictor:

```r
summary(lm(mpg ~ cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.6036754
```

```r
summary(lm(mpg ~ displacement, data=Auto))$adj.r.squared
```

```
## [1] 0.6473274
```

```r
summary(lm(mpg ~ horsepower, data=Auto))$adj.r.squared
```

```
## [1] 0.6049379
```

```r
summary(lm(mpg ~ weight, data=Auto))$adj.r.squared
```

```
## [1] 0.6918423
```

```r
summary(lm(mpg ~ acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.1771025
```

```r
summary(lm(mpg ~ year, data=Auto))$adj.r.squared
```

```
## [1] 0.3353279
```

```r
summary(lm(mpg ~ origin, data=Auto))$adj.r.squared
```

```
## [1] 0.317716
```

```r
r2 <- 0.6918 # Adjusted R^2 from 'weight'
```

Now doing the same but with two predictors:

```r
summary(lm(mpg ~ weight + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.695908
```

```r
summary(lm(mpg ~ weight + displacement, data=Auto))$adj.r.squared
```

```
## [1] 0.6974191
```

```r
summary(lm(mpg ~ weight + horsepower, data=Auto))$adj.r.squared
```

```
## [1] 0.7048656
```

```r
summary(lm(mpg ~ weight + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.698154
```

```r
summary(lm(mpg ~ weight + year, data=Auto))$adj.r.squared
```

```
## [1] 0.8071941
```

```r
summary(lm(mpg ~ weight + origin, data=Auto))$adj.r.squared
```

```
## [1] 0.7004287
```

```r
r2 <- .8072 # from weight + year
```

Now with three predictors:

```r
summary(lm(mpg ~ weight + year + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8069069
```

```r
summary(lm(mpg ~ weight + year + displacement, data=Auto))$adj.r.squared
```

```
## [1] 0.8066989
```

```r
summary(lm(mpg ~ weight + year + horsepower, data=Auto))$adj.r.squared
```

```
## [1] 0.8068368
```

```r
summary(lm(mpg ~ weight + year + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8071393
```

```r
summary(lm(mpg ~ weight + year + origin, data=Auto))$adj.r.squared
```

```
## [1] 0.8160407
```

```r
r2 <- 0.816 # from weight + year + origin
```

Four predictors:

```r
summary(lm(mpg ~ weight + year + origin + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8155716
```

```r
summary(lm(mpg ~ weight + year + origin + displacement, data=Auto))$adj.r.squared
```

```
## [1] 0.8162176
```

```r
summary(lm(mpg ~ weight + year + origin + horsepower, data=Auto))$adj.r.squared
```

```
## [1] 0.8161764
```

```r
summary(lm(mpg ~ weight + year + origin + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.816203
```

```r
r2 <- 0.8162 # From any of the last three.
```

Five predictors:

```r
summary(lm(mpg ~ weight + year + origin + displacement + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8165646
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower, data=Auto))$adj.r.squared
```

```
## [1] 0.8176929
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8175491
```

```r
r2 <- 0.8177 # weight + year + origin + displacement + horsepower
```

Six predictors:

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8183822
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8175962
```

```r
r2 <- 0.8184 # weight + year + origin + displacement + horsepower + cylinders
```

All predictors: 

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower + cylinders + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8182238
```

This final model gives an $R^2$ value of 0.8182 which is  not larger than the previous $R^2$ value of .8184. 

Thus, using the forward model selection process we get the predictors **Weight, Year, Origin, Displacement, Horsepower, and Cylinders** yielding the highest $R^2$ value via the multiple linear regression.

### Part (b)
####Backward model selection

All predictors:

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower + cylinders + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8182238
```

```r
r2 <- .8182
```

Six predictors:

```r
summary(lm(mpg ~ year + origin + displacement + horsepower + cylinders + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.7721514
```

```r
summary(lm(mpg ~ weight + year + displacement + horsepower + cylinders + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8062826
```

```r
summary(lm(mpg ~ weight + year + origin + horsepower + cylinders + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8153867
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + cylinders + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8179822
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower + acceleration, data=Auto))$adj.r.squared
```

```
## [1] 0.8175962
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8183822
```

```r
r2 <- 0.8184 # weight + year + origin + displacement + horsepower + cylinders
```

Five Predictors:

```r
summary(lm(mpg ~ weight + year + origin + displacement + horsepower, data=Auto))$adj.r.squared
```

```
## [1] 0.8176929
```

```r
summary(lm(mpg ~ weight + year + origin + displacement + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8165646
```

```r
summary(lm(mpg ~ weight + year + origin + horsepower + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.8157239
```

```r
summary(lm(mpg ~ weight + year + displacement + horsepower + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.806434
```

```r
summary(lm(mpg ~ weight + origin + displacement + horsepower + cylinders, data=Auto))$adj.r.squared
```

```
## [1] 0.7169503
```

None of these have a higher adjusted $R^2$ value than when we used six predictors, so I believe we can stop here.

This means the predictors **Weight, Year, Origin, Displacement, Horsepower, and Cylinders** yield the highest $R^2$ value of 0.8184.
