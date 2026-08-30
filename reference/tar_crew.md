# Get crew worker info.

For the most recent run of the pipeline with
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
where a `crew` controller was started, get summary-level information of
the workers.

## Usage

``` r
tar_crew(store = targets::tar_config_get("store"))
```

## Arguments

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

A data frame one row per `crew` controller (potentially multiple rows if
`tar_option_get("controller")` is a controller group) and the following
columns:

- `controller`: name of the `crew` controller.

- `targets`: number of times the controller attempted to run a target.

- `seconds`: number of seconds the workers spent running tasks.

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
[`tar_pid()`](https://docs.ropensci.org/targets/reference/tar_pid.md),
[`tar_process()`](https://docs.ropensci.org/targets/reference/tar_process.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
if (requireNamespace("crew", quietly = TRUE)) {
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(controller = crew::crew_controller_local())
  list(
    tar_target(x, seq_len(2)),
    tar_target(y, 2 * x, pattern = map(x))
  )
}, ask = FALSE)
tar_make()
tar_process()
tar_process(pid)
}
})
}
```
