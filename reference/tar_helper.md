# Write a helper R script.

Write a helper R script for a `targets` pipeline. Could be supporting
functions or the target script file (default: `_targets.R`) itself.

`tar_helper()` expects an unevaluated expression for the `code`
argument, whereas `tar_helper_raw()` expects an evaluated expression
object.

## Usage

``` r
tar_helper(path = NULL, code = NULL, tidy_eval = TRUE, envir = parent.frame())

tar_helper_raw(path = NULL, code = NULL)
```

## Arguments

- path:

  Character of length 1, path to write (or overwrite) `code`. If the
  parent directory does not exist, `tar_helper_raw()` creates it.
  `tar_helper()` overwrites the file if it already exists.

- code:

  Code to write to `path`. `tar_helper()` expects an unevaluated
  expression for the `code` argument, whereas `tar_helper_raw()` expects
  an evaluated expression object.

- tidy_eval:

  Logical, whether to use tidy evaluation on `code`. If turned on, you
  can substitute expressions and symbols using `!!` and `!!!`. See
  examples below.

- envir:

  Environment for tidy evaluation.

## Value

`NULL` (invisibly)

## Details

`tar_helper()` is a specialized version of
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)
with flexible paths and tidy evaluation.

## See also

Other scripts:
[`tar_edit()`](https://docs.ropensci.org/targets/reference/tar_edit.md),
[`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md),
[`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md),
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)

## Examples

``` r
# Without tidy evaluation:
path <- tempfile()
tar_helper(path, code = x <- 1)
tar_helper_raw(path, code = quote(x <- 1)) # equivalent
writeLines(readLines(path))
#> x <- 1
# With tidy evaluation:
y <- 123
tar_helper(path, x <- !!y)
writeLines(readLines(path))
#> x <- 123
```
