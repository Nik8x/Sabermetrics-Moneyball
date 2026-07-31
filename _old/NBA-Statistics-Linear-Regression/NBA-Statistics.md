NBA-Statistics-Linear-Regression
================
Niket

``` r
# Read in the data
NBA = read.csv("NBA_train.csv")
str(NBA)
```

    ## 'data.frame':    835 obs. of  20 variables:
    ##  $ SeasonEnd: int  1980 1980 1980 1980 1980 1980 1980 1980 1980 1980 ...
    ##  $ Team     : Factor w/ 37 levels "Atlanta Hawks",..: 1 2 5 6 8 9 10 11 12 13 ...
    ##  $ Playoffs : int  1 1 0 0 0 0 0 1 0 1 ...
    ##  $ W        : int  50 61 30 37 30 16 24 41 37 47 ...
    ##  $ PTS      : int  8573 9303 8813 9360 8878 8933 8493 9084 9119 8860 ...
    ##  $ oppPTS   : int  8334 8664 9035 9332 9240 9609 8853 9070 9176 8603 ...
    ##  $ FG       : int  3261 3617 3362 3811 3462 3643 3527 3599 3639 3582 ...
    ##  $ FGA      : int  7027 7387 6943 8041 7470 7596 7318 7496 7689 7489 ...
    ##  $ X2P      : int  3248 3455 3292 3775 3379 3586 3500 3495 3551 3557 ...
    ##  $ X2PA     : int  6952 6965 6668 7854 7215 7377 7197 7117 7375 7375 ...
    ##  $ X3P      : int  13 162 70 36 83 57 27 104 88 25 ...
    ##  $ X3PA     : int  75 422 275 187 255 219 121 379 314 114 ...
    ##  $ FT       : int  2038 1907 2019 1702 1871 1590 1412 1782 1753 1671 ...
    ##  $ FTA      : int  2645 2449 2592 2205 2539 2149 1914 2326 2333 2250 ...
    ##  $ ORB      : int  1369 1227 1115 1307 1311 1226 1155 1394 1398 1187 ...
    ##  $ DRB      : int  2406 2457 2465 2381 2524 2415 2437 2217 2326 2429 ...
    ##  $ AST      : int  1913 2198 2152 2108 2079 1950 2028 2149 2148 2123 ...
    ##  $ STL      : int  782 809 704 764 746 783 779 782 900 863 ...
    ##  $ BLK      : int  539 308 392 342 404 562 339 373 530 356 ...
    ##  $ TOV      : int  1495 1539 1684 1370 1533 1742 1492 1565 1517 1439 ...

``` r
# How many wins to make the playoffs?
table(NBA$W, NBA$Playoffs)
```

    ##     
    ##       0  1
    ##   11  2  0
    ##   12  2  0
    ##   13  2  0
    ##   14  2  0
    ##   15 10  0
    ##   16  2  0
    ##   17 11  0
    ##   18  5  0
    ##   19 10  0
    ##   20 10  0
    ##   21 12  0
    ##   22 11  0
    ##   23 11  0
    ##   24 18  0
    ##   25 11  0
    ##   26 17  0
    ##   27 10  0
    ##   28 18  0
    ##   29 12  0
    ##   30 19  1
    ##   31 15  1
    ##   32 12  0
    ##   33 17  0
    ##   34 16  0
    ##   35 13  3
    ##   36 17  4
    ##   37 15  4
    ##   38  8  7
    ##   39 10 10
    ##   40  9 13
    ##   41 11 26
    ##   42  8 29
    ##   43  2 18
    ##   44  2 27
    ##   45  3 22
    ##   46  1 15
    ##   47  0 28
    ##   48  1 14
    ##   49  0 17
    ##   50  0 32
    ##   51  0 12
    ##   52  0 20
    ##   53  0 17
    ##   54  0 18
    ##   55  0 24
    ##   56  0 16
    ##   57  0 23
    ##   58  0 13
    ##   59  0 14
    ##   60  0  8
    ##   61  0 10
    ##   62  0 13
    ##   63  0  7
    ##   64  0  3
    ##   65  0  3
    ##   66  0  2
    ##   67  0  4
    ##   69  0  1
    ##   72  0  1

``` r
# Compute Points Difference
NBA$PTSdiff = NBA$PTS - NBA$oppPTS
```

``` r
# Check for linear relationship
plot(NBA$PTSdiff, NBA$W)
```

![](NBA-Statistics_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-4-1.png)

``` r
# Linear regression model for wins
WinsReg = lm(W ~ PTSdiff, data=NBA)
summary(WinsReg)
```

    ## 
    ## Call:
    ## lm(formula = W ~ PTSdiff, data = NBA)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -9.7393 -2.1018 -0.0672  2.0265 10.6026 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 4.100e+01  1.059e-01   387.0   <2e-16 ***
    ## PTSdiff     3.259e-02  2.793e-04   116.7   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 3.061 on 833 degrees of freedom
    ## Multiple R-squared:  0.9423, Adjusted R-squared:  0.9423 
    ## F-statistic: 1.361e+04 on 1 and 833 DF,  p-value: < 2.2e-16

VIDEO 3
=======

Linear regression model for points scored
=========================================

PointsReg = lm(PTS ~ X2PA + X3PA + FTA + AST + ORB + DRB + TOV + STL + BLK, data=NBA) summary(PointsReg)

Sum of Squared Errors
=====================

PointsReg*r**e**s**i**d**u**a**l**s**S**S**E* = *s**u**m*(*P**o**i**n**t**s**R**e**g*residuals^2) SSE

Root mean squared error
=======================

RMSE = sqrt(SSE/nrow(NBA)) RMSE

Average number of points in a season
====================================

mean(NBA$PTS)

Remove insignifcant variables
=============================

summary(PointsReg)

PointsReg2 = lm(PTS ~ X2PA + X3PA + FTA + AST + ORB + DRB + STL + BLK, data=NBA) summary(PointsReg2)

PointsReg3 = lm(PTS ~ X2PA + X3PA + FTA + AST + ORB + STL + BLK, data=NBA) summary(PointsReg3)

PointsReg4 = lm(PTS ~ X2PA + X3PA + FTA + AST + ORB + STL, data=NBA) summary(PointsReg4)

Compute SSE and RMSE for new model
==================================

SSE\_4 = sum(PointsReg4$residuals^2) RMSE\_4 = sqrt(SSE\_4/nrow(NBA)) SSE\_4 RMSE\_4

VIDEO 4
=======

Read in test set
================

NBA\_test = read.csv("NBA\_test.csv")

Make predictions on test set
============================

PointsPredictions = predict(PointsReg4, newdata=NBA\_test)

Compute out-of-sample R^2
=========================

SSE = sum((PointsPredictions - NBA\_test*P**T**S*)<sup>2</sup>)*S**S**T* = *s**u**m*((*m**e**a**n*(*N**B**A*PTS) - NBA\_test$PTS)^2) R2 = 1 - SSE/SST R2

Compute the RMSE
================

RMSE = sqrt(SSE/nrow(NBA\_test)) RMSE
