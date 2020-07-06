## Resubmission

### Second

- Changed `installed.packages()` using `find.package()` to speed up 
  the evaluation. In the `README`'s example, `installed.packages()` is
  still used because that example have to know all the 
  names of the installed packages.

- Changed `\dontrun{}` with `\donttest{}` in the examples which can 
  "run" but should not be tested. Telegram's related examples and the
  `use_ui()` one still use `\dontrun{}` because they require special
  environments (Telegram configuration and package-like project) to be
  executed without errors.
  
> Note: on `rhub::check_for_cran()` an additional failure happen because
of missing `{data.table}` (that is imported by `{Hmisc}` which is
`library()`ed into some examples). On the other hand `{depigner}` never
used `{data.table}`, hence I did not put it on Suggests neighter (note
also that I tried to do so and the error persists anyway claiming that
`{data.table}` has been required but not available). Anyway, I suppose
it is an issue with the rhub environment: I get no error using
[`devtools::check_win_devel()`](https://win-builder.r-project.org/7M0tU8X02QGt),
nor locally (Debian 10 + Win 10), nor on [GitHub
Actions](https://github.com/CorradoLanera/depigner/actions/runs/147626935)
on win (release), ubuntu (oldrle + release), macOS (release).


### First

This is a resubmission. In this version I have not change anything:

The error:

    Found the following (possibly) invalid URLs:
      URL: https://elisasovrano.it
        From: README.md
        Status: Error
        Message: libcurl error code 6:
             Could not resolve host: elisasovrano.it

It was caused by the fact that https://elisasovrano.it had not yet been activated. It is now, and the link is working properly.




## Test environments

### provided by
- usethis::use_tidy_github_actions()
- devtools::check()
- devtools::check(remote = TRUE, manual = TRUE)
- devtools::check_win_devel()
- rhub::check_for_cran()


### Configurations
- local:
  * Debian 10 (Linux 4.19.0), R 4.0.1
  * Windows 10 pro, R 4.0.1

- remote:
  * MacOS 10.15.5: R 4.0.1, R-devel
  * Ubuntu Linux 16.04 LTS: R 4.0.1, R-oldrel, GCC
  * Fedora Linux, R-devel, clang, gfortran
  * Windows 10.0.17763: R 4.0.1
  * Windows Server 2008 R2 SP1: R-devel, 32/64 bit




## R CDM check results

0 errors ✓ | 0 warnings ✓ | 2 notes x

* Possibly mis-spelled words in DESCRIPTION:
     Pigna (9:14)
     Pignas (2:49)
     pìn'n'a (9:22)
     
_Pigna_ is the Italian word that gives the name to the package.
_Pignas_ would be the English-like plural for Pigna.
_pìn'n'a_ is the phonetic writing to know how to correctly pronunciate
Pigna (given that it is not an English word).


* This is a new release.


### Allert: isolate r-hub error
Error for lack of system dependencies on r-hub only for
  * Ubuntu Linux 16.04 LTS: R 4.0.1, GCC
  
With the same configuration called from a GitHub Action defined like in
https://github.com/r-lib/actions/blob/master/examples/check-full.yaml,
it succeded (https://github.com/CorradoLanera/depigner/pull/14).
  