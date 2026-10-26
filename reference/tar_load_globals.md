# Load globals for debugging, testing, and prototyping

Load user-defined packages, functions, global objects, and settings
defined in the target script file (default: `_targets.R`). This function
is for debugging, testing, and prototyping only. It is not recommended
for use inside a serious pipeline or to report the results of a serious
pipeline.

## Usage

``` r
tar_load_globals(
  envir = parent.frame(),
  script = targets::tar_config_get("script")
)
```

## Arguments

- envir:

  Environment to source the target script (default: `_targets.R`).
  Defaults to the calling environment.

- script:

  Character of length 1, path to the target script file that defines the
  pipeline (`_targets.R` by default). This path should be either an
  absolute path or a path relative to the project root where you will
  call
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and other functions. When
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and friends run the script from the current working directory. If the
  argument `NULL`, the setting is not modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

## Value

`NULL` (invisibly).

## Details

This function first sources the target script file (default:
`_targets.R`) to loads all user-defined functions, global objects, and
settings into the current R process. Then, it loads all the packages
defined in `tar_option_get("packages")` (default: `(.packages())`) using
[`library()`](https://rdrr.io/r/base/library.html) with `lib.loc`
defined in `tar_option_get("library")` (default: `NULL`).

## Storage access

Several functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md),
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
[`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md),
and
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
read or modify the local data store of the pipeline. The local data
store is in flux while a pipeline is running, and depending on how
distributed computing or cloud computing is set up, not all targets can
even reach it. So please do not call these functions from inside a
target as part of a running pipeline. The only exception is literate
programming target factories in the `tarchetypes` package such as
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.html)
and
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.html).

## See also

Other debug:
[`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md),
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md),
[`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md),
[`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(packages = "callr")
  analyze_data <- function(data) {
    summary(data)
  }
  list(
    tar_target(x, 1 + 1),
    tar_target(y, 1 + 1)
  )
}, ask = FALSE)
tar_load_globals()
print(analyze_data)
print("callr" %in% (.packages()))
})
}
```
