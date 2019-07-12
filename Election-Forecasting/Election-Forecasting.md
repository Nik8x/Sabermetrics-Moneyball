Election-Forecasting
================
Niket

``` r
polling <- read.csv(file.choose(), header = TRUE)
str(polling)
```

    ## 'data.frame':    145 obs. of  7 variables:
    ##  $ State     : Factor w/ 50 levels "Alabama","Alaska",..: 1 1 2 2 3 3 3 4 4 4 ...
    ##  $ Year      : int  2004 2008 2004 2008 2004 2008 2012 2004 2008 2012 ...
    ##  $ Rasmussen : int  11 21 NA 16 5 5 8 7 10 NA ...
    ##  $ SurveyUSA : int  18 25 NA NA 15 NA NA 5 NA NA ...
    ##  $ DiffCount : int  5 5 1 6 8 9 4 8 5 2 ...
    ##  $ PropR     : num  1 1 1 1 1 ...
    ##  $ Republican: int  1 1 1 1 1 1 1 1 1 1 ...

``` r
table(polling$Year)
```

    ## 
    ## 2004 2008 2012 
    ##   50   50   45

``` r
summary(polling)
```

    ##          State          Year        Rasmussen          SurveyUSA       
    ##  Arizona    :  3   Min.   :2004   Min.   :-41.0000   Min.   :-33.0000  
    ##  Arkansas   :  3   1st Qu.:2004   1st Qu.: -8.0000   1st Qu.:-11.7500  
    ##  California :  3   Median :2008   Median :  1.0000   Median : -2.0000  
    ##  Colorado   :  3   Mean   :2008   Mean   :  0.0404   Mean   : -0.8243  
    ##  Connecticut:  3   3rd Qu.:2012   3rd Qu.:  8.5000   3rd Qu.:  8.0000  
    ##  Florida    :  3   Max.   :2012   Max.   : 39.0000   Max.   : 30.0000  
    ##  (Other)    :127                  NA's   :46         NA's   :71        
    ##    DiffCount           PropR          Republican    
    ##  Min.   :-19.000   Min.   :0.0000   Min.   :0.0000  
    ##  1st Qu.: -6.000   1st Qu.:0.0000   1st Qu.:0.0000  
    ##  Median :  1.000   Median :0.6250   Median :1.0000  
    ##  Mean   : -1.269   Mean   :0.5259   Mean   :0.5103  
    ##  3rd Qu.:  4.000   3rd Qu.:1.0000   3rd Qu.:1.0000  
    ##  Max.   : 11.000   Max.   :1.0000   Max.   :1.0000  
    ## 

``` r
# there are decent number of missing values
```

``` r
# Install and load mice package
# install.packages("mice")
library(mice)
```

    ## Warning: package 'mice' was built under R version 3.4.3

    ## Loading required package: lattice

``` r
# Multiple imputation
simple <- polling[c("Rasmussen", "SurveyUSA", "PropR", "DiffCount")]
summary(simple)
```

    ##    Rasmussen          SurveyUSA            PropR          DiffCount      
    ##  Min.   :-41.0000   Min.   :-33.0000   Min.   :0.0000   Min.   :-19.000  
    ##  1st Qu.: -8.0000   1st Qu.:-11.7500   1st Qu.:0.0000   1st Qu.: -6.000  
    ##  Median :  1.0000   Median : -2.0000   Median :0.6250   Median :  1.000  
    ##  Mean   :  0.0404   Mean   : -0.8243   Mean   :0.5259   Mean   : -1.269  
    ##  3rd Qu.:  8.5000   3rd Qu.:  8.0000   3rd Qu.:1.0000   3rd Qu.:  4.000  
    ##  Max.   : 39.0000   Max.   : 30.0000   Max.   :1.0000   Max.   : 11.000  
    ##  NA's   :46         NA's   :71

``` r
set.seed(144)
imputed <- complete(mice(simple))
```

    ## 
    ##  iter imp variable
    ##   1   1  Rasmussen  SurveyUSA
    ##   1   2  Rasmussen  SurveyUSA
    ##   1   3  Rasmussen  SurveyUSA
    ##   1   4  Rasmussen  SurveyUSA
    ##   1   5  Rasmussen  SurveyUSA
    ##   2   1  Rasmussen  SurveyUSA
    ##   2   2  Rasmussen  SurveyUSA
    ##   2   3  Rasmussen  SurveyUSA
    ##   2   4  Rasmussen  SurveyUSA
    ##   2   5  Rasmussen  SurveyUSA
    ##   3   1  Rasmussen  SurveyUSA
    ##   3   2  Rasmussen  SurveyUSA
    ##   3   3  Rasmussen  SurveyUSA
    ##   3   4  Rasmussen  SurveyUSA
    ##   3   5  Rasmussen  SurveyUSA
    ##   4   1  Rasmussen  SurveyUSA
    ##   4   2  Rasmussen  SurveyUSA
    ##   4   3  Rasmussen  SurveyUSA
    ##   4   4  Rasmussen  SurveyUSA
    ##   4   5  Rasmussen  SurveyUSA
    ##   5   1  Rasmussen  SurveyUSA
    ##   5   2  Rasmussen  SurveyUSA
    ##   5   3  Rasmussen  SurveyUSA
    ##   5   4  Rasmussen  SurveyUSA
    ##   5   5  Rasmussen  SurveyUSA

``` r
summary(imputed)
```

    ##    Rasmussen         SurveyUSA           PropR          DiffCount      
    ##  Min.   :-41.000   Min.   :-33.000   Min.   :0.0000   Min.   :-19.000  
    ##  1st Qu.: -8.000   1st Qu.:-11.000   1st Qu.:0.0000   1st Qu.: -6.000  
    ##  Median :  3.000   Median :  1.000   Median :0.6250   Median :  1.000  
    ##  Mean   :  1.731   Mean   :  1.517   Mean   :0.5259   Mean   : -1.269  
    ##  3rd Qu.: 11.000   3rd Qu.: 18.000   3rd Qu.:1.0000   3rd Qu.:  4.000  
    ##  Max.   : 39.000   Max.   : 30.000   Max.   :1.0000   Max.   : 11.000

``` r
polling$Rasmussen <- imputed$Rasmussen
polling$SurveyUSA <- imputed$SurveyUSA
summary(polling)
```

    ##          State          Year        Rasmussen         SurveyUSA      
    ##  Arizona    :  3   Min.   :2004   Min.   :-41.000   Min.   :-33.000  
    ##  Arkansas   :  3   1st Qu.:2004   1st Qu.: -8.000   1st Qu.:-11.000  
    ##  California :  3   Median :2008   Median :  3.000   Median :  1.000  
    ##  Colorado   :  3   Mean   :2008   Mean   :  1.731   Mean   :  1.517  
    ##  Connecticut:  3   3rd Qu.:2012   3rd Qu.: 11.000   3rd Qu.: 18.000  
    ##  Florida    :  3   Max.   :2012   Max.   : 39.000   Max.   : 30.000  
    ##  (Other)    :127                                                     
    ##    DiffCount           PropR          Republican    
    ##  Min.   :-19.000   Min.   :0.0000   Min.   :0.0000  
    ##  1st Qu.: -6.000   1st Qu.:0.0000   1st Qu.:0.0000  
    ##  Median :  1.000   Median :0.6250   Median :1.0000  
    ##  Mean   : -1.269   Mean   :0.5259   Mean   :0.5103  
    ##  3rd Qu.:  4.000   3rd Qu.:1.0000   3rd Qu.:1.0000  
    ##  Max.   : 11.000   Max.   :1.0000   Max.   :1.0000  
    ## 

``` r
# Subset data into training set and test set
Train <- subset(polling, Year == 2004 | Year == 2008)
Test <- subset(polling, Year == 2012)
```

``` r
# Smart Baseline
table(Train$Republican)
```

    ## 
    ##  0  1 
    ## 47 53

``` r
sign(20)
```

    ## [1] 1

``` r
sign(-10)
```

    ## [1] -1

``` r
sign(0)
```

    ## [1] 0

``` r
table(sign(Train$Rasmussen))
```

    ## 
    ## -1  0  1 
    ## 42  3 55

``` r
table(Train$Republican, sign(Train$Rasmussen))
```

    ##    
    ##     -1  0  1
    ##   0 42  2  3
    ##   1  0  1 52

``` r
# this is a better model 
```

``` r
# Multicollinearity
str(Train)
```

    ## 'data.frame':    100 obs. of  7 variables:
    ##  $ State     : Factor w/ 50 levels "Alabama","Alaska",..: 1 1 2 2 3 3 4 4 5 5 ...
    ##  $ Year      : int  2004 2008 2004 2008 2004 2008 2004 2008 2004 2008 ...
    ##  $ Rasmussen : int  11 21 16 16 5 5 7 10 -11 -27 ...
    ##  $ SurveyUSA : int  18 25 21 21 15 8 5 9 -11 -24 ...
    ##  $ DiffCount : int  5 5 1 6 8 9 8 5 -8 -5 ...
    ##  $ PropR     : num  1 1 1 1 1 1 1 1 0 0 ...
    ##  $ Republican: int  1 1 1 1 1 1 1 1 0 0 ...

``` r
cor(Train[c("Rasmussen", "SurveyUSA", "PropR", "DiffCount", "Republican")])
```

    ##            Rasmussen SurveyUSA     PropR DiffCount Republican
    ## Rasmussen  1.0000000 0.9194508 0.8404803 0.5124098  0.8021191
    ## SurveyUSA  0.9194508 1.0000000 0.8756581 0.5541816  0.8205806
    ## PropR      0.8404803 0.8756581 1.0000000 0.8273785  0.9484204
    ## DiffCount  0.5124098 0.5541816 0.8273785 1.0000000  0.8092777
    ## Republican 0.8021191 0.8205806 0.9484204 0.8092777  1.0000000

``` r
# Logistic Regression Model
mod1 <- glm(Republican ~ PropR, data = Train, family = "binomial")
summary(mod1)
```

    ## 
    ## Call:
    ## glm(formula = Republican ~ PropR, family = "binomial", data = Train)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -2.22880  -0.06541   0.10260   0.10260   1.37392  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)   -6.146      1.977  -3.108 0.001882 ** 
    ## PropR         11.390      3.153   3.613 0.000303 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 138.269  on 99  degrees of freedom
    ## Residual deviance:  15.772  on 98  degrees of freedom
    ## AIC: 19.772
    ## 
    ## Number of Fisher Scoring iterations: 8

``` r
# Training set predictions
pred1 <- predict(mod1, type = "response")
table(Train$Republican, pred1 >= 0.5)
```

    ##    
    ##     FALSE TRUE
    ##   0    45    2
    ##   1     2   51

``` r
# Two-variable model
mod2 <- glm(Republican ~ SurveyUSA + DiffCount, data = Train, family = "binomial")
pred2 <- predict(mod2, type = "response")
table(Train$Republican, pred2 >= 0.5)
```

    ##    
    ##     FALSE TRUE
    ##   0    45    2
    ##   1     1   52

``` r
summary(mod2)
```

    ## 
    ## Call:
    ## glm(formula = Republican ~ SurveyUSA + DiffCount, family = "binomial", 
    ##     data = Train)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -2.01196  -0.00698   0.01005   0.05074   1.54975  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)  -1.1405     1.2456  -0.916   0.3599  
    ## SurveyUSA     0.2976     0.1949   1.527   0.1267  
    ## DiffCount     0.7673     0.4188   1.832   0.0669 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 138.269  on 99  degrees of freedom
    ## Residual deviance:  12.439  on 97  degrees of freedom
    ## AIC: 18.439
    ## 
    ## Number of Fisher Scoring iterations: 10

``` r
# Smart baseline accuracy
table(Test$Republican, sign(Test$Rasmussen))
```

    ##    
    ##     -1  0  1
    ##   0 18  2  4
    ##   1  0  0 21

``` r
# Test set predictions
TestPrediction <- predict(mod2, newdata = Test, type = "response")
table(Test$Republican, TestPrediction >= 0.5)
```

    ##    
    ##     FALSE TRUE
    ##   0    23    1
    ##   1     0   21

``` r
# Analyze mistake
subset(Test, TestPrediction >= 0.5 & Republican == 0)
```

    ##      State Year Rasmussen SurveyUSA DiffCount     PropR Republican
    ## 24 Florida 2012         2         0         6 0.6666667          0
