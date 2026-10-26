# Write a target script file.

The `tar_script()` function is a convenient way to create the required
target script file (default: `_targets.R`) in the current working
directory. It always overwrites the existing target script, and it
requires you to be in the working directory where you intend to write
the file, so be careful. See the "Target script" section for details.

## Usage

``` r
tar_script(
  code = NULL,
  library_targets = TRUE,
  ask = NULL,
  script = targets::tar_config_get("script")
)
```

## Arguments

- code:

  R code to write to the target script file. If `NULL`, an example
  target script file is written instead.

- library_targets:

  logical, whether to write a
  [`library(targets)`](https://docs.ropensci.org/targets/) line at the
  top of the target script file automatically (recommended). If `TRUE`,
  you do not need to explicitly put
  [`library(targets)`](https://docs.ropensci.org/targets/) in `code`.

- ask:

  Logical, whether to ask before writing if the target script file
  already exists. If `NULL`, defaults to `Sys.getenv("TAR_ASK")`. (Set
  to `"true"` or `"false"` with
  [`Sys.setenv()`](https://rdrr.io/r/base/Sys.setenv.html)). If `ask`
  and the `TAR_ASK` environment variable are both indeterminate,
  defaults to
  [`interactive()`](https://rdrr.io/r/base/interactive.html).

- script:

  Character of length 1, where to write the target script file. Defaults
  to `tar_config_get("script")`, which in turn defaults to `_targets.R`.

## Value

`NULL` (invisibly).

## Target script file

Every `targets` project requires a target script file. The target script
file is usually a file called `_targets.R` Functions
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
and friends look for the target script and run it to set up the pipeline
just prior to the main task. Every target script file should run the
following steps in the order below:

1.  Package: load the `targets` package. This step is automatically
    inserted at the top of the target script file produced by
    `tar_script()` if `library_targets` is `TRUE`, so you do not need to
    explicitly include it in `code`.

2.  Globals: load custom functions and global objects into memory.
    Usually, this section is a bunch of calls to
    [`source()`](https://rdrr.io/r/base/source.html) that run scripts
    defining user-defined functions. These functions support the R
    commands of the targets.

3.  Options: call
    [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
    to set defaults for targets-specific settings such as the names of
    required packages. Even if you have no specific options to set, it
    is still recommended to call
    [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
    in order to register the proper environment.

4.  Targets: define one or more target definition objects using
    [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

5.  Pipeline: call [`list()`](https://rdrr.io/r/base/list.html) to
    aggregate the targets from (4) into a list. Every target script file
    must return a pipeline object, which usually means ending with a
    call to [`list()`](https://rdrr.io/r/base/list.html). In
    practice, (4) and (5) can be combined together in the same function
    call.

## See also

Other scripts:
[`tar_edit()`](https://docs.ropensci.org/targets/reference/tar_edit.md),
[`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md),
[`tar_helper()`](https://docs.ropensci.org/targets/reference/tar_helper.md),
[`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md)

## Examples

``` r
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script() # Writes an example target script file.
# Writes a user-defined target script:
tar_script({
  library(targets)
  library(tarchetypes)
  x <- tar_target(x, 1 + 1)
  tar_option_set()
  list(x)
}, ask = FALSE)
writeLines(readLines("_targets.R"))
})
#> library(targets)
#> library(targets)
#> library(tarchetypes)
#> x <- tar_target(x, 1 + 1)
#> tar_option_set()
#> list(x)
```
