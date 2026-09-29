# Get main process info.

Get info on the most recent main R process to orchestrate the targets of
the current project.

## Usage

``` r
tar_process(names = NULL, store = targets::tar_config_get("store"))
```

## Arguments

- names:

  Optional, names of the data points to return. If supplied,
  `tar_process()` returns only the rows of the names you select. The
  object supplied to `names` should be `NULL` or a `tidyselect`
  expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

- store:

  Character of length 1, path to the `targets` data store. Defaults to
  `tar_config_get("store")`, which in turn defaults to `_targets/`. When
  you set this argument, the value of `tar_config_get("store")` is
  temporarily changed for the current function call. See
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about how to set the data store path persistently for a
  project.

## Value

A data frame with metadata on the most recent main R process to
orchestrate the targets of the current project. The output includes the
`pid` of the main process.

## Details

The main process is the R process invoked by
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
or similar. If `callr_function` is not `NULL`, this is an external
process, and the `pid` in the return value will not agree with
[`Sys.getpid()`](https://rdrr.io/r/base/Sys.getpid.html) in your current
interactive session. The process may or may not be alive. You may want
to check the status with `tar_pid() %in% ps::ps_pids()` before running
another call to
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
for the same project.

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

Other data:
[`tar_crew()`](https://docs.ropensci.org/targets/reference/tar_crew.md),
[`tar_pid()`](https://docs.ropensci.org/targets/reference/tar_pid.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(x, seq_len(2)),
    tar_target(y, 2 * x, pattern = map(x))
  )
}, ask = FALSE)
tar_make()
tar_process()
tar_process(pid)
})
}
```
