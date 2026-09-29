# Repeatedly poll progress in the R console.

Print the information in
[`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md)
at regular intervals.

## Usage

``` r
tar_poll(
  interval = 1,
  timeout = Inf,
  fields = c("skipped", "dispatched", "completed", "errored", "canceled", "since"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- interval:

  Number of seconds to wait between iterations of polling progress.

- timeout:

  How many seconds to run before exiting.

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

`NULL` (invisibly). Called for its side effects.

## See also

Other progress:
[`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md),
[`tar_completed()`](https://docs.ropensci.org/targets/reference/tar_completed.md),
[`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md),
[`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md),
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md),
[`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md),
[`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md),
[`tar_skipped()`](https://docs.ropensci.org/targets/reference/tar_skipped.md),
[`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
[`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md),
[`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  list(
    tar_target(x, seq_len(100)),
    tar_target(y, Sys.sleep(0.1), pattern = map(x))
  )
}, ask = FALSE)
px <- tar_make(callr_function = callr::r_bg, reporter = "silent")
tar_poll()
})
}
```
