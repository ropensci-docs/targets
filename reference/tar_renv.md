# Set up package dependencies for compatibility with `renv`

Write package dependencies to a script file (by default, named
`_targets_packages.R` in the root project directory). Each package is
written to a separate line as a standard
[`library()`](https://rdrr.io/r/base/library.html) call (e.g.
[`library(package)`](https://rdrr.io/r/base/library.html)) so `renv` can
identify them automatically. The following packages are included:

    * Packages listed in the `packages` argument of [tar_target()]
      or [tar_option_set()].
    * Packages required by the `format` set in [tar_target()]
      or [tar_option_set()].
    * Packages directly listed in the `extras` argument of [tar_renv()].

## Usage

``` r
tar_renv(
  extras = c("bslib", "crew", "gt", "markdown", "rstudioapi", "shiny", "shinybusy",
    "shinyWidgets", "visNetwork"),
  path = "_targets_packages.R",
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function),
  envir = parent.frame(),
  script = targets::tar_config_get("script")
)
```

## Arguments

- extras:

  Character vector of additional packages to declare as project
  dependencies.

- path:

  Character of length 1, path to the script file to populate with
  [`library()`](https://rdrr.io/r/base/library.html) calls.

- callr_function:

  A function from `callr` to start a fresh clean R process to do the
  work. Set to `NULL` to run in the current session instead of an
  external process (but restart your R session just before you do in
  order to clear debris out of the global environment). `callr_function`
  needs to be `NULL` for interactive debugging, e.g.
  `tar_option_set(debug = "your_target")`. However, `callr_function`
  should not be `NULL` for serious reproducible work.

- callr_arguments:

  A list of arguments to `callr_function`.

- envir:

  An environment, where to run the target R script (default:
  `_targets.R`) if `callr_function` is `NULL`. Ignored if
  `callr_function` is anything other than `NULL`. `callr_function`
  should only be `NULL` for debugging and testing purposes, not for
  serious runs of a pipeline, etc.

  The `envir` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and related functions always overrides the current value of
  `tar_option_get("envir")` in the current R session just before running
  the target script file, so whenever you need to set an alternative
  `envir`, you should always set it with
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  from within the target script file. In other words, if you call
  `tar_option_set(envir = envir1)` in an interactive session and then
  `tar_make(envir = envir2, callr_function = NULL)`, then `envir2` will
  be used.

- script:

  Character of length 1, path to the target script file. Defaults to
  `tar_config_get("script")`, which in turn defaults to `_targets.R`.
  When you set this argument, the value of `tar_config_get("script")` is
  temporarily changed for the current function call. See
  [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md),
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about the target script file and how to set it
  persistently for a project.

## Value

Nothing, invisibly.

## Details

This function gets called for its side-effect, which writes package
dependencies to a script for compatibility with `renv`. The generated
file should **not** be edited by hand and will be overwritten each time
`tar_renv()` is called.

The behavior of `renv` is to create and manage a project-local `R`
library and keep a record of project dependencies in a file called
`renv.lock`. To identify dependencies, `renv` crawls through code to
find packages explicitly mentioned using
[`library()`](https://rdrr.io/r/base/library.html),
[`require()`](https://rdrr.io/r/base/library.html), or `::`. However,
`targets` pipelines introduce extra package dependencies through the
`packages` and `format` arguments of
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
and
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).
`tar_renv()` writes a special R script which allows these extra packages
(along with packages listed in `extras`) to be detected by `renv`.

With the script written by `tar_renv()`, `renv` is able to crawl the
file to identify package dependencies (with `renv::dependencies()`).
`tar_renv()` only serves to make your `targets` project compatible with
`renv`, it is still the users responsibility to call `renv::init()` and
`renv::snapshot()` directly to initialize and manage a project-local `R`
library. This allows your `targets` pipeline to have its own
self-contained `R` library separate from your standard `R` library. See
<https://rstudio.github.io/renv/index.html> for more information.

## Performance

If you use `renv`, then overhead from project initialization could slow
down
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
and friends. If you experience slowness, please make sure your `renv`
library is on a fast file system. (For example, slow network drives can
severely reduce performance.) In addition, you can disable the slowest
`renv` initialization checks. After confirming at
<https://rstudio.github.io/renv/reference/config.html> that you can
safely disable these checks, you can write lines
`RENV_CONFIG_SANDBOX_ENABLED=false` and
`RENV_CONFIG_SYNCHRONIZED_CHECK=false` in your user-level `.Renviron`
file. If you disable the synchronization check, remember to call
`renv::status()` periodically to check the health of your `renv` project
library.

## See also

<https://rstudio.github.io/renv/articles/renv.html>

Other scripts:
[`tar_edit()`](https://docs.ropensci.org/targets/reference/tar_edit.md),
[`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md),
[`tar_helper()`](https://docs.ropensci.org/targets/reference/tar_helper.md),
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)

## Examples

``` r
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
  tar_script({
    library(targets)
    library(tarchetypes)
    tar_option_set(packages = c("tibble", "qs"))
    list()
  }, ask = FALSE)
  tar_renv()
  writeLines(readLines("_targets_packages.R"))
})
#> Warning message:
#> package ‘tarchetypes’ was built under R version 4.6.1 
#> # Generated by targets::tar_renv(): do not edit by hand
#> library(bslib)
#> library(crew)
#> library(gt)
#> library(markdown)
#> library(qs)
#> library(rstudioapi)
#> library(shiny)
#> library(shinyWidgets)
#> library(shinybusy)
#> library(tibble)
#> library(visNetwork)
tar_option_reset()
```
