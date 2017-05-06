---
layout: page
title: Regression
permalink: /linreg/
---

QR and SVD factorization for rank deficient least squares
=================================================

Ref: [Pollard](http://www.stat.yale.edu/~pollard/Courses/312.fall2016/Handouts/QR.pdf).

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

The lm function in R uses QR factorization. When rank deficient (e.g. X will have a null space) we can still use QR as above and get most general solution, or we can use SVD to compute psuedoinverse which computes the min-norm solution for . The QR general solution in the rank deficient case shows that the easiest specific solution is to set the p-r components of to zero, which is what R does. See [Lambers](www.math.usm.edu/lambers/mat610/sum10/lecture11.pdf). To get the min norm solution from the QR factorization imposes conditions that are related to the pseudo inverse as explained, but the SVD provides an orthogonal decomposition of the regression coefficient vector into *natural* subspace components  Range(Transpose(X)) and Nullspace(X) (by natural we mean that provided by the fundamental thm of linear algebra for any matrix X). Thus we can use it (via pseudoinverse) to get minimum norm solution (i.e. choosing the specific solution where we have zero contribution from the null-space component of the regression coefficient vector).

Ridge and Lasso Regression, i.e. Regularization
=================================================
An alternative to OLS is to use regularization.  The norm type (e.g. L2 or L1) provide different types of solutions or shrinkage (decreasing the norm of the estimated coefficient vector).  Ridge (L2 regularization) regression also helps rank-deficiency by ensuring singular values which are zero, get 'lifted'.  

Lasso uses an L1 regularization which creates sparse solutions.  

Here's a picture from Witten et. al. showing the geometry of L2 and L1 regularization:
