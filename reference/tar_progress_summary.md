# Summarize target progress.

Summarize the progress of a run of the pipeline.

## Usage

``` r
tar_progress_summary(
  fields = c("skipped", "dispatched", "completed", "errored", "canceled", "since"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- fields:

  Optional character vector of names of progress data columns to read.
  Set to `NULL` to read all fields.

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

A data frame with one row and the following optional columns that can be
selected with `fields`. (`time` is omitted by default.)

- `dispatched`: number of targets that were sent off to run and did not
  (yet) finish. These targets may not actually be running, depending on
  the status and workload of parallel workers.

- `completed`: number of targets that completed without error or
  cancellation.

- `errored`: number of targets that threw an error.

- `canceled`: number of canceled targets (see
  [`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md)).

- `since`: how long ago progress last changed (`Sys.time() - time`).

- `time`: the time when the progress last changed (modification
  timestamp of the `_targets/meta/progress` file).

## See also

Other progress:
[`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md),
[`tar_completed()`](https://docs.ropensci.org/targets/reference/tar_completed.md),
[`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md),
[`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md),
[`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md),
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md),
[`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md),
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
    tar_target(z, stopifnot(y < 1.5), pattern = map(y), error = "continue")
  )
}, ask = FALSE)
try(tar_make())
tar_progress_summary()
})
}
```
