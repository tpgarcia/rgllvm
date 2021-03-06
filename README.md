---
output: github_document
---

<!-- README.md is generated from README.Rmd. Please edit that file -->


# rgllvm

<!-- badges: start -->
<!-- badges: end -->


This R package implements a semiparametric approach to estimate parameters for repeated measures data using a generalized linear latent variable model where the latent variable is a random intercept or random slope and the distribution of the latent variable is left unspecified. The corresponding references are:

Garcia, T.P. and Ma, Y. (2015). Optimal estimator for
logistic model with distribution-free random intercept.
Scandinavian Journal of Statistics, 43, 156-171.

Ma, Y. & Genton, M. G. (2010). Explicit estimating equations for semiparametric generalized
linear latent variable models. Journal of the Royal Statistical Society, Series B 72, 475-495.

Wei, Y., Ma, Y., Garcia, T.P. and Sinha, S. (2019). Consistent estimator
for logistic mixed effect models. The Canadian Journal of Statistics, 47, 140-156. doi:10.1002/cjs.11482.



## Installation

You can install the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("tpgarcia/rgllvm", force=TRUE)
```
## Example

Here are examples of the approach when the latent variable is a random intercept and random slope. 


```r
library(rgllvm)

###########################################
##                                       ##
## EXAMPLE 1: Random intercept example   ##
##                                       ##
###########################################


# Setup parameters to generate the data
set.seed(1)  ## set the random seed generator.
n = 500 ## sample size
m = rep(3,n) ## number of repeats per subject
beta0 = c(0.5,0,-0.5) ## true beta value
p = length(beta0) ## number of beta values
fr.form = "normal"  ## distribution of the random intercept
fxr.form = "uniform"  ## distribution of the covariate 

## produce long form of the repeated measures data
data.set.out <- gendata(beta0=beta0,n,m=rep(3,n),fr.form,fxr.form,
                        latent.variable.type = "intercept",
                    center.x=FALSE)

## extract the data to input into rgllvm
y.data <- data.set.out[,c("family","Y")]
x.data <- data.set.out[,c("family",paste0("X",1:p))]
z.data <- data.set.out[,c("family","Z")]

output <- rgllvm(y.data,x.data,z.data,
                   n,m,p,
                   beta0=rep(0,p),
                   family="binomial") 


###########################################
##                                       ##
## EXAMPLE 2: Random slope example       ##
##                                       ##
###########################################


# Setup parameters to generate the data
set.seed(1)  ## set the random seed generator.
n = 500 ## sample size
m = rep(3,n) ## number of repeats per subject
beta0 = c(0.35,0.6,-0.4) ## true beta value
p = length(beta0) ## number of beta values
fr.form = "normal"  ## distribution of the random intercept
fxr.form = "uniform"  ## distribution of the covariate 

## produce long form of the repeated measures data
data.set.out <- gendata(beta0=beta0,n,m=rep(3,n),fr.form,fxr.form,
                        latent.variable.type = "slope",
                    center.x=FALSE)

## extract the data to input into rgllvm
y.data <- data.set.out[,c("family","Y")]
x.data <- data.set.out[,c("family",paste0("X",1:p))]
z.data <- data.set.out[,c("family","Z")]

output <- rgllvm(y.data,x.data,z.data,
                   n,m,p,
                   beta0=rep(0,p),
                   family="binomial")   
```
