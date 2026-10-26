# Tabulate the progress of dynamic branches.

Read a project's target progress data for the most recent run of the
pipeline and display the tabulated status of dynamic branches. Only the
most recent record is shown.

## Usage

``` r
tar_progress_branches(
  names = NULL,
  fields = NULL,
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Optional, names of the targets. If supplied,
  [`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
  only returns progress information on these targets. The object
  supplied to `names` should be `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- fields:

  Optional, names of progress data columns to read. Set to `NULL` to
  read all fields.

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

A data frame with one row per target per progress status and the
following columns.

- `name`: name of the pattern.

- `progress`: progress status: `"dispatched"`, `"completed"`,
  `"canceled"`, or `"errored"`.

- `branches`: number of branches in the progress category.

- `total`: total number of branches planned for the whole pattern.
  Values within the same pattern should all be equal.

## See also

Other progress:
[`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md),
[`tar_completed()`](https://docs.ropensci.org/targets/reference/tar_completed.md),
[`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md),
[`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md),
[`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md),
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md),
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
    tar_target(y, x, pattern = map(x)),
    tar_target(z, stopifnot(y < 1.5), pattern = map(y))
  )
}, ask = FALSE)
try(tar_make())
tar_progress_branches()
})
}
```
