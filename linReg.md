---
layout: page
title: Regression
permalink: /linreg/
---



R Markdown
----------

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

``` r
summary(cars)
```

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

Including Plots
---------------

You can also embed plots, for example:

![](../linReg_files/figure-markdown_github/pressure-1.png)

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

QR factorization for rank deficient least squares
=================================================

This is from [Pollard](http://www.stat.yale.edu/~pollard/Courses/312.fall2016/Handouts/QR.pdf).

Lets create linearly dependent data matrix X

``` r
set.seed(10)   # for reproducibility
mydata <- data.frame(y=rnorm(10),
    x1=1:10,x2= 11:20, x3= 0.5*(1:10)-3*(11:20))
out <- lm(y ~ ., data=mydata)
summary(out)
```

    ##
    ## Call:
    ## lm(formula = y ~ ., data = mydata)
    ##
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max
    ## -1.0211 -0.5231  0.1832  0.4320  0.9085
    ##
    ## Coefficients: (2 not defined because of singularities)
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) -0.18175    0.49193  -0.369    0.721
    ## x1          -0.05616    0.07928  -0.708    0.499
    ## x2                NA         NA      NA       NA
    ## x3                NA         NA      NA       NA
    ##
    ## Residual standard error: 0.7201 on 8 degrees of freedom
    ## Multiple R-squared:  0.05903,    Adjusted R-squared:  -0.05859
    ## F-statistic: 0.5019 on 1 and 8 DF,  p-value: 0.4988

The lm function in R uses QR factorization. When rank deficient (e.g. X will have a null space) we can still use QR as above and get most general solution, or we can use SVD to compute psuedoinverse which computes the min-norm solution for . The QR general solution in the rank deficient case shows that the easiest specific solution is to set the p-r components of to zero, which is what R does. See [Lambers](www.math.usm.edu/lambers/mat610/sum10/lecture11.pdf). To get the min norm solution from the QR factorization imposes conditions that are related to the pseudo inverse as explained, but the SVD provides an orthogonal decomposition of the regression coefficient vector into subspace components  R(X^T) and N(X). thus we can use it (via pseudoinverse) to get min norm (e.g. choosing the specific solution where we have zero contribution from the null-space component).
