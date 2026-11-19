# List completed targets.

List targets whose progress is `"completed"`.

## Usage

``` r
tar_completed(names = NULL, store = targets::tar_config_get("store"))
```

## Arguments

- names:

  Optional, names of the targets. If supplied, the output is restricted
  to the selected targets. The object supplied to `names` should be
  `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

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

A character vector of completed targets.

## See also

Other progress:
[`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md),
[`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md),
[`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md),
[`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md),
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md),
[`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md),
[`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md),
[`tar_skipped()`](https://docs.ropensci.org/targets/reference/tar_skipped.md),
[`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
[`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md),
[`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)

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
tar_completed()
tar_completed(starts_with("y_")) # see also any_of()
})
}
```
