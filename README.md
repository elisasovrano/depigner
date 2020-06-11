
<!-- README.md is generated from README.Rmd. Please edit that file -->

# depigner [![](https://img.shields.io/badge/WEB%20site-click%20me-orange.svg)](https://corradolanera.github.io/depigner/) <img src='man/figures/logo.png' align="right" height="138.5" />

A utility package to help you deal with ***pigne***

<!-- badges: start -->

|                 |                                                                                                                                                   |                                                                                                                                                                  |                                                                                                                                                  |
| :-------------- | :-----------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------: |
| **Development** |          [![Devel version](https://img.shields.io/badge/devel%20version-0.7.0.9000-blue.svg)](https://github.com/CorradoLanera/depigner)          |                     [![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)                      | [![last commit](https://img.shields.io/github/last-commit/CorradoLanera/depigner.svg)](https://github.com/CorradoLanera/depigner/commits/master) |
| **CRAN**        |                 [![CRAN status](https://www.r-pkg.org/badges/version/depigner)](https://cran.r-project.org/package=depigner) yet…                 |                  [![downloads](http://cranlogs.r-pkg.org/badges/grand-total/depigner?color=blue)](https://cran.r-project.org/package=depigner)                   |           [![downloads](http://cranlogs.r-pkg.org/badges/last-month/depigner?color=blue)](https://cran.r-project.org/package=depigner)           |
| **CI**          | [![R build status](https://github.com/CorradoLanera/depigner/workflows/R-CMD-check/badge.svg)](https://github.com/CorradoLanera/depigner/actions) | [![Coverage status](https://codecov.io/gh/CorradoLanera/depigner/branch/master/graph/badge.svg)](https://codecov.io/github/CorradoLanera/depigner?branch=master) |     [![code size](https://img.shields.io/github/languages/code-size/CorradoLanera/depigner.svg)](https://github.com/CorradoLanera/depigner)      |

<!-- badges: end -->

> **Pigna** \[*pìn’n’a*\] is the Italian word for *pine cone*.\[1\] In
> jargon, it’s used to identify something (like a task…) boring, banal,
> annoying, painful, frustrating and maybe even with a not so beautiful
> or rewarding result, just like the obstinate act of trying to
> challenge yourself in extracting pine nuts from a pine cone, provided
> that at the end you will find at least one inside it…

# Overview

This package aims to provide some useful functions to be used to solve
small everyday problems of coding or analyzing data with R. The hope is
to provide solutions to that kind of problems which would be normally
solved using quick-and-dirty (ugly and maybe even wrong) patches.

| Tools Category                           | Function(s)               | Aim                                                                      |
| :--------------------------------------- | :------------------------ | :----------------------------------------------------------------------- |
| [Harrell’s verse](#harrells-verse-tools) | `tidy_summary()`          | *`pander`-ready* data frame from `Hmisc::sumamry()`                      |
|                                          | `paired_test_continuous`  | Paired test for continuous variable into `Hmisc::summary`                |
|                                          | `paired_test_categorical` | Paired test for categorical variable into `Hmisc::summary`               |
|                                          | `adjust_p()`              | Adjusts P-values for multiplicity of tests at `tidy_summary()`           |
|                                          | `summary_interact()`      | data frame of OR for interaction from `rms::lrm()`                       |
|                                          | `Htypes()`                | Will be your variables continuous or categorical in `Hmisc::describe()`? |
| [Statistical](#statistical-tools)        | `ci2p()`                  | Get P-value form estimation and confidence interval                      |
| [Programming](#programming-tools)        | `pb_len()`                | Quick set-up of a `progress::progress_bar()` progress bar                |
|                                          | `install_pkg_set()`       | Politely install set of packages (topic-related sets at `?pkg_sets`)     |
| [Development](#development-tools)        | `use_ui()`                | Activate `{usethis}` user interface into your own package                |
|                                          | `please_install()`        | Politely ask the user to install a package                               |
|                                          | `imported_from()`         | List packages imported from a package (which has to be installed)        |
| [Telegram](#telegram-tools)              | `start_bot_for_chat()`    | Quick start of a `{telegram.bot}` Telegram’s bot                         |
|                                          | `send_to_telegram()`      | Unified wrapper to send *someRthing* to a Telegram chat                  |
|                                          | `errors_to_telegram()`    | Divert all your error messages from the console to a Telegram chat       |
| [Why not?\!](#why-not)                   | `gdp()`                   | Do you have TOO much pignas in your back?\! … try this out ;-)           |

# Installation

You can install the development version from
[GitHub](https://github.com/) calling:

``` r
# install.packages("devtools")
devtools::install_github("CorradoLanera/depigner")
```

Next, you can attach it to your session by:

``` r
library(depigner)
#> Welcome to depigner: we are here to un-stress you!
#> 
```

# Provided Tools

## Harrell’s Verse Tools

  - **`tidy_summary()`**: produces a data frame from the `summary()`
    functions provided by `{Hmisc}` (Harrell Jr, Charles Dupont, and
    others. 2020) and `{rms}` (Harrell Jr 2020) packages ready to be
    `pander::pander()`ed (Daróczi and Tsegelskyi 2018).

Currently it is tested for method *reverse* only:

``` r
library(rms)
#> Loading required package: Hmisc
#> Loading required package: lattice
#> Loading required package: survival
#> Loading required package: Formula
#> Loading required package: ggplot2
#> 
#> Attaching package: 'Hmisc'
#> The following objects are masked from 'package:base':
#> 
#>     format.pval, units
#> Loading required package: SparseM
#> 
#> Attaching package: 'SparseM'
#> The following object is masked from 'package:base':
#> 
#>     backsolve
  options(datadist = 'dd')
library(survival)
library(pander)

dd <- datadist(iris)
my_summary <- summary(Species ~., data = iris, method = "reverse")
tidy_summary(my_summary) %>% 
  pander()
#> Warning in seq.default(along = mat.names.width): partial argument match of
#> 'along' to 'along.with'
```

|   |   setosa (N=50)   | versicolor (N=50) | virginica (N=50)  |
| :-: | :---------------: | :---------------: | :---------------: |
| 2 | 4.800/5.000/5.200 | 5.600/5.900/6.300 | 6.225/6.500/6.900 |
| 3 | 3.200/3.400/3.675 | 2.525/2.800/3.000 | 2.800/3.000/3.175 |
| 4 | 1.400/1.500/1.575 | 4.000/4.350/4.600 | 5.100/5.550/5.875 |
| 5 |    0.2/0.2/0.3    |    1.2/1.3/1.5    |    1.8/2.0/2.3    |

``` r


dd <- datadist(heart)
surv <- Surv(heart$start, heart$stop, heart$event)
f    <- cph(surv ~ age + year + surgery, data = heart)
#> Warning in seq.default(length = n): partial argument match of 'length' to
#> 'length.out'
#> Warning in seq.default(1L, by = 2L, length = lastrow.length): partial argument
#> match of 'length' to 'length.out'
#> Warning in seq.default(1L, by = 2L, length = start): partial argument match of
#> 'length' to 'length.out'

#> Warning in seq.default(1L, by = 2L, length = start): partial argument match of
#> 'length' to 'length.out'

#> Warning in seq.default(1L, by = 2L, length = start): partial argument match of
#> 'length' to 'length.out'

#> Warning in seq.default(1L, by = 2L, length = start): partial argument match of
#> 'length' to 'length.out'

#> Warning in seq.default(1L, by = 2L, length = start): partial argument match of
#> 'length' to 'length.out'

#> Warning in seq.default(1L, by = 2L, length = start): partial argument match of
#> 'length' to 'length.out'
my_summary <- summary(f)
#> Warning: partial match of 'coef' to 'coefficients'
tidy_summary(my_summary) %>% 
  pander()
```

|         | Diff. |   HR   | Lower 95% CI | Upper 95% CI |
| :-----: | :---: | :----: | :----------: | :----------: |
|   age   | 10.69 | 1.336  |    1.009     |    1.767     |
|  year   | 3.37  | 0.6104 |    0.3831    |    0.9727    |
| surgery |   1   | 0.5286 |    0.2574    |    1.085     |

  - **`paired_test_*()`**: Paired test for categorical/continuous
    variables to be used in the `summary()` of the `{Hmisc}` (Harrell
    Jr, Charles Dupont, and others. 2020) package:

<!-- end list -->

``` r
data(Arthritis)
# categorical -------------------------
## two groups
summary(Treatment ~ Sex,
    data    = Arthritis,
    method  = "reverse",
    test    = TRUE,
    catTest = paired_test_categorical
)
#> 
#> 
#> Descriptive Statistics by Treatment
#> Warning in seq.default(along = mat.names.width): partial argument match of
#> 'along' to 'along.with'
#> +----------+--------------------+--------------------+------------------------------+
#> |          |Placebo             |Treated             |  Test                        |
#> |          |(N=43)              |(N=41)              |Statistic                     |
#> +----------+--------------------+--------------------+------------------------------+
#> |Sex : Male|           26%  (11)|           34%  (14)|Chi-square=5.92 d.f.=1 P=0.015|
#> +----------+--------------------+--------------------+------------------------------+
## more than two groups
summary(Improved ~ Sex,
    data    = Arthritis,
    method  = "reverse",
    test    = TRUE,
    catTest = paired_test_categorical
)
#> 
#> 
#> Descriptive Statistics by Improved
#> Warning in seq.default(along = mat.names.width): partial argument match of
#> 'along' to 'along.with'
#> +----------+---------------+---------------+---------------+---------------------+
#> |          |None           |Some           |Marked         |  Test               |
#> |          |(N=42)         |(N=14)         |(N=28)         |Statistic            |
#> +----------+---------------+---------------+---------------+---------------------+
#> |Sex : Male|      40%  (17)|      14%  ( 2)|      21%  ( 6)|t=1.03 d.f.=2 P=0.412|
#> +----------+---------------+---------------+---------------+---------------------+

# continuous --------------------------
## two groups
summary(Species ~.,
    data    = iris[iris$Species != "setosa",],
    method  = "reverse",
    test    = TRUE,
    conTest = paired_test_continuous
)
#> 
#> 
#> Descriptive Statistics by Species
#> Warning in seq.default(along = mat.names.width): partial argument match of
#> 'along' to 'along.with'
#> +------------+---------------------+---------------------+------------------------+
#> |            |versicolor           |virginica            |  Test                  |
#> |            |(N=50)               |(N=50)               |Statistic               |
#> +------------+---------------------+---------------------+------------------------+
#> |Sepal.Length|    5.600/5.900/6.300|    6.225/6.500/6.900| t=-5.28 d.f.=49 P<0.001|
#> +------------+---------------------+---------------------+------------------------+
#> |Sepal.Width |    2.525/2.800/3.000|    2.800/3.000/3.175| t=-3.08 d.f.=49 P=0.003|
#> +------------+---------------------+---------------------+------------------------+
#> |Petal.Length|    4.000/4.350/4.600|    5.100/5.550/5.875|t=-12.09 d.f.=49 P<0.001|
#> +------------+---------------------+---------------------+------------------------+
#> |Petal.Width |       1.2/1.3/1.5   |       1.8/2.0/2.3   |t=-14.69 d.f.=49 P<0.001|
#> +------------+---------------------+---------------------+------------------------+
## more than two groups
summary(Species ~.,
    data    = iris,
    method  = "reverse",
    test    = TRUE,
    conTest = paired_test_continuous
)
#> 
#> 
#> Descriptive Statistics by Species
#> Warning in seq.default(along = mat.names.width): partial argument match of
#> 'along' to 'along.with'
#> +------------+--------------------+--------------------+--------------------+-----------------------+
#> |            |setosa              |versicolor          |virginica           |  Test                 |
#> |            |(N=50)              |(N=50)              |(N=50)              |Statistic              |
#> +------------+--------------------+--------------------+--------------------+-----------------------+
#> |Sepal.Length|   4.800/5.000/5.200|   5.600/5.900/6.300|   6.225/6.500/6.900| F=30.55 d.f.=2 P<0.001|
#> +------------+--------------------+--------------------+--------------------+-----------------------+
#> |Sepal.Width |   3.200/3.400/3.675|   2.525/2.800/3.000|   2.800/3.000/3.175| F=12.63 d.f.=2 P<0.001|
#> +------------+--------------------+--------------------+--------------------+-----------------------+
#> |Petal.Length|   1.400/1.500/1.575|   4.000/4.350/4.600|   5.100/5.550/5.875|F=322.89 d.f.=2 P<0.001|
#> +------------+--------------------+--------------------+--------------------+-----------------------+
#> |Petal.Width |      0.2/0.2/0.3   |      1.2/1.3/1.5   |      1.8/2.0/2.3   |F=234.21 d.f.=2 P<0.001|
#> +------------+--------------------+--------------------+--------------------+-----------------------+
```

  - **`adjust_p()`**: Adjust P-values of a `tidy_summary` objects:

<!-- end list -->

``` r
my_summary <- summary(Species ~., data = iris, method = "reverse")

  tidy_summary(my_summary) %>%
      adjust_p()
#> Warning in seq.default(along = mat.names.width): partial argument match of
#> 'along' to 'along.with'
#> Warning: Unknown or uninitialised column: `P-value`.
#> x The object `x` does not have a P-value column.
#>   Have you select `test = TRUE` in the `summary` call?
#> x `x` is returned without changes.
#> # A tibble: 4 x 4
#>   `&nbsp;` `setosa \n(N=50)`   `versicolor \n(N=50)` `virginica \n(N=50)`
#>   <chr>    <chr>               <chr>                 <chr>               
#> 1 2        "4.800/5.000/5.200" "5.600/5.900/6.300"   "6.225/6.500/6.900" 
#> 2 3        "3.200/3.400/3.675" "2.525/2.800/3.000"   "2.800/3.000/3.175" 
#> 3 4        "1.400/1.500/1.575" "4.000/4.350/4.600"   "5.100/5.550/5.875" 
#> 4 5        "   0.2/0.2/0.3"    "   1.2/1.3/1.5"      "   1.8/2.0/2.3"
```

  - **`summary_interact()`**: Produce a data frame of OR (with the
    corresponding CI95%) for the interactions between different
    combination of a continuous variable (for which it is possible to
    define the reference and the target values) and (every or a
    selection of levels of) a categorical one in a logistic model
    provided by `lrm()` (from the `{rms}` package (Harrell Jr 2020)):

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

  - **`Htypes()`** and friends: get/check types of variable with respect
    to the `{Hmisc}` ecosystem (Harrell Jr, Charles Dupont, and others.
    2020).

<!-- end list -->

``` r
Htypes(mtcars)
#>    mpg    cyl   disp     hp   drat     wt   qsec     vs     am   gear   carb 
#>  "con" "none"  "con"  "con"  "con"  "con"  "con"  "cat"  "cat" "none" "none"

desc <- Hmisc::describe(mtcars)
Htypes(desc)
#>    mpg    cyl   disp     hp   drat     wt   qsec     vs     am   gear   carb 
#>  "con" "none"  "con"  "con"  "con"  "con"  "con"  "cat"  "cat" "none" "none"
Htype(desc[[1]])
#> [1] "con"
is_Hcat(desc[[1]])
#> [1] FALSE
is_Hcon(desc[[1]])
#> [1] TRUE
```

## Statistical Tools

  - **`ci2p()`**: compute the p-value related with a provided confidence
    interval:

<!-- end list -->

``` r
ci2p(1.125, 0.634,  1.999, log_transform = TRUE)
#> [1] 0.367902
```

## Programming Tools

  - **`pb_len()`**: Progress bar of given length, wrapper from the
    `{progress}` (Csárdi and FitzJohn 2019) package:

<!-- end list -->

``` r
pb <- pb_len(100)

for (i in 1:100) {
    Sys.sleep(0.1)
    tick(pb, paste("i = ", i))
}
```

  - **`install_pkg_set()`**: Simple and polite wrapper to install sets
    of packages. Moreover, `{depigner}` provides some sets already
    defined for common scenario in R (analyses, production, documenting,
    …). See them by call `?pgk_sets`.

<!-- end list -->

``` r
install_pkg_set() # this install the whole `?pkg_all`
install_pkg_set(pkg_stan)

?pkg_sets
```

## Development Tools

  - **`use_ui()`**: Use `{usethis}`’ user interface (Wickham and Bryan
    2020) in your package

<!-- end list -->

``` r
# in the initial setup steps of the development of a package
use_ui()
```

  - **`lease_install()`**: This is a polite wrapper to
    `install.packages()` inspired (= w/ very minimal modification) by a
    function Hadley showed us during a course.

<!-- end list -->

``` r
a_pkg_i_miss <- setdiff(available.packages(), installed.packages())[[1]]
please_install(a_pkg_i_miss)
```

  - **`imported_from()`**: If you would like to know which packages are
    imported by a package (eg to know which packages are required for
    its installation or either installed during it) you can use this
    function

<!-- end list -->

``` r
imported_from("depigner")
#>  [1] "broom"        "desc"         "dplyr"        "fs"           "ggplot2"     
#>  [6] "Hmisc"        "magrittr"     "progress"     "purrr"        "rlang"       
#> [11] "rprojroot"    "stats"        "stringr"      "telegram.bot" "tibble"      
#> [16] "tidyr"        "usethis"      "utils"
```

## Telegram Tools

  - **Wrappers to simple use of Telegram’s bots**: wrappers from the
    `{telegram.bot}` package (Benedito 2019):

<!-- end list -->

``` r
# Set up a Telegram bot. read `?start_bot_for_chat`
start_bot_for_chat()

# Send something to telegram
send_to_telegram("hello world")

library(ggplot2)
gg <- ggplot(mtcars, aes(x = mpg, y = hp, colour = cyl)) +
    geom_point()
send_to_telegram(
  "following an `mtcars` coloured plot",
  parse_mode = "Markdown"
)
send_to_telegram(gg)

# Divert output errors to the telegram bot
errors_to_telegram()
```

## Why Not?\!

  - **`gdp()`**: A wrapper to relax

<!-- end list -->

``` r
gdp(7)
```

# Feature request

If you need some more features, please open an issue
[here](https://github.com/CorradoLanera/depigner/issues).

# Bug reports

If you encounter a bug, please file a
[reprex](https://github.com/tidyverse/reprex) (minimal reproducible
example) [here](https://github.com/CorradoLanera/depigner/issues).

# Code of Conduct

Please note that the depigner project is released with a [Contributor
Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.

<!--=================================================================-->

# Reference

<div id="refs" class="references">

<div id="ref-R-telegram.bot">

Benedito, Ernest. 2019. *Telegram.bot: Develop a ’Telegram Bot’ with R*.
<https://CRAN.R-project.org/package=telegram.bot>.

</div>

<div id="ref-R-progress">

Csárdi, Gábor, and Rich FitzJohn. 2019. *Progress: Terminal Progress
Bars*. <https://CRAN.R-project.org/package=progress>.

</div>

<div id="ref-R-pander">

Daróczi, Gergely, and Roman Tsegelskyi. 2018. *Pander: An R ’Pandoc’
Writer*. <https://CRAN.R-project.org/package=pander>.

</div>

<div id="ref-R-rms">

Harrell Jr, Frank E. 2020. *Rms: Regression Modeling Strategies*.
<https://CRAN.R-project.org/package=rms>.

</div>

<div id="ref-R-Hmisc">

Harrell Jr, Frank E, with contributions from Charles Dupont, and many
others. 2020. *Hmisc: Harrell Miscellaneous*.
<https://CRAN.R-project.org/package=Hmisc>.

</div>

<div id="ref-R-usethis">

Wickham, Hadley, and Jennifer Bryan. 2020. *Usethis: Automate Package
and Project Setup*.

</div>

</div>

1.  You can find all the possible meanings of *pigna*
    [here](http://www.treccani.it/vocabolario/pigna/), and you can
    listen how to pronounce it
    [here](http://www.dizionario.rai.it/poplemma.aspx?lid=27556&r=43587).
    Note: the Italian plural for “pigna” is “pigne” \[*pìn’n’e*\].
