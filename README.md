depigner: package of utilities for *pignas*.
================

<!-- README.md is generated from README.Rmd. Please edit that file -->

[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![CRAN
status](https://www.r-pkg.org/badges/version/depigner)](https://cran.r-project.org/package=depigner)
[![Travis build
status](https://travis-ci.org/CorradoLanera/depigner.svg?branch=master)](https://travis-ci.org/CorradoLanera/depigner)
[![Coverage
status](https://codecov.io/gh/CorradoLanera/depigner/branch/master/graph/badge.svg)](https://codecov.io/github/CorradoLanera/depigner?branch=master)

## Overview

This package collect usefull function frequently used at the Unit of
Biostatistics, Epidemiology and Public Health of the Department of
Cardiac, Thoracic and Vascular sciences at the university of Padova.

## Install

You can install the development version from
[GitHub](https://github.com/) with the following procedure:

``` r
# install.packages("devtools")
devtools::install_github("CorradoLanera/depigner")
```

## Provided function

  - **`summary_interact()`**: produce a data frame of OR (with the
    corresponding CI95%) for the interactions between different
    combination of a continuous variable (for which it is possible to
    define the reference and the target values) and (every or a
    selection of levels of) a categorical one in a logistic model
    provided by `lrm()` (from the `rms` package (Harrell, Jr. 2018)).

<!-- end list -->

``` r
summary_interact(lrm_mod, age, abo) %>%
  pander()
```

|          | Low | High | Diff. | Odds Ratio | Lower 95% CI | Upper 95% CI |
| :------: | :-: | :--: | :---: | :--------: | :----------: | :----------: |
| age - A  | 43  |  58  |  15   |   1.002    |    0.557     |    1.802     |
| age - B  | 43  |  58  |  15   |   1.817    |     0.74     |    4.463     |
| age - AB | 43  |  58  |  15   |   0.635    |    0.186     |    2.169     |
| age - O  | 43  |  58  |  15   |   0.645    |    0.352     |    1.182     |

``` r

summary_interact(lrm_mod, age, abo, p = TRUE) %>%
  pander()
```

|          | Low | High | Diff. | Odds Ratio | Lower 95% CI | Upper 95% CI | P-value |
| :------: | :-: | :--: | :---: | :--------: | :----------: | :----------: | :-----: |
| age - A  | 43  |  58  |  15   |   1.002    |    0.557     |    1.802     |  0.498  |
| age - B  | 43  |  58  |  15   |   1.817    |     0.74     |    4.463     |  0.137  |
| age - AB | 43  |  58  |  15   |   0.635    |    0.186     |    2.169     |  0.728  |
| age - O  | 43  |  58  |  15   |   0.645    |    0.352     |    1.182     |  0.883  |

  - **`tidy_summary()`**: produces a data frame from the `summary()`
    functions provided by **Hmisc** and **rms** packages. At the moment
    it is tested only for method *reverse*

<!-- end list -->

``` r
dd <- datadist(iris)
my_summary <- summary(Species ~., data = iris, method = "reverse")
tidy_summary(my_summary) %>% 
  pander()
```

|              |   setosa (N=50)   | versicolor (N=50) | virginica (N=50)  |
| :----------: | :---------------: | :---------------: | :---------------: |
| Sepal.Length | 4.800/5.000/5.200 | 5.600/5.900/6.300 | 6.225/6.500/6.900 |
| Sepal.Width  | 3.200/3.400/3.675 | 2.525/2.800/3.000 | 2.800/3.000/3.175 |
| Petal.Length | 1.400/1.500/1.575 | 4.000/4.350/4.600 | 5.100/5.550/5.875 |
| Petal.Width  |    0.2/0.2/0.3    |    1.2/1.3/1.5    |    1.8/2.0/2.3    |

``` r


dd <- datadist(heart)
surv <- Surv(heart$start, heart$stop, heart$event)
f    <- cph(surv ~ age + year + surgery, data = heart)
my_summary <- summary(f)
tidy_summary(my_summary) %>% 
  pander()
```

|         | Diff. |   HR   | Lower 95% CI | Upper 95% CI |
| :-----: | :---: | :----: | :----------: | :----------: |
|   age   | 10.69 | 1.336  |    1.009     |    1.767     |
|  year   | 3.37  | 0.6104 |    0.3831    |    0.9727    |
| surgery |   1   | 0.5286 |    0.2574    |    1.085     |

## Provided data

  - **`ubesp_pkg`**: main packages uses at UBESP

## Feature request

If you need some more features, please open an issue on
[github](https://github.com/CorradoLanera/depigner/issues).

## Bug reports

If you encounter a bug, please file a
[reprex](https://github.com/tidyverse/reprex) (minimal reproducible
example) on [github](https://github.com/CorradoLanera/depigner/issues).

## Code of Conduct

Please note that the ‘depigner’ project is released with a [Contributor
Code of Conduct](.github/CODE_OF_CONDUCT.md). By contributing to this
project, you agree to abide by its
terms.

<!--=========================================================================-->

## Reference

<div id="refs" class="references">

<div id="ref-R-rms">

Harrell, Jr., Frank E. 2018. *Rms: Regression Modeling Strategies*.
<https://CRAN.R-project.org/package=rms>.

</div>

</div>
