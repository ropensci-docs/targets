# List old targets

List all the targets whose last successful run occurred before a certain
point in time. Combine with
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md),
you can use `tar_older()` to automatically rerun targets at regular
intervals. See the examples for a demonstration.

## Usage

``` r
tar_older(
  time,
  names = NULL,
  inclusive = FALSE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- time:

  A `POSIXct` object of length 1, time threshold. Targets older than
  this time stamp are returned. For example, if
  `time = Sys.time() - as.difftime(1, units = "weeks")` then
  `tar_older()` returns targets older than one week ago.

- names:

  Names of eligible targets. Targets excluded from `names` will not be
  returned even if they are old. The object supplied to `names` should
  be `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- inclusive:

  Logical of length 1, whether to include targets completed at exactly
  the `time` given.

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

A character vector of names of old targets with recorded timestamp
metadata.

## Details

Only applies to targets with recorded time stamps: just non-branching
targets and individual dynamic branches. As of `targets` version 0.6.0,
these time stamps are available for these targets regardless of storage
format. Earlier versions of `targets` do not record time stamps for
remote storage such as `format = "url"` or `repository = "aws"` in
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## See also

Other time:
[`tar_newer()`](https://docs.ropensci.org/targets/reference/tar_newer.md),
[`tar_timestamp()`](https://docs.ropensci.org/targets/reference/tar_timestamp.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(tar_target(x, seq_len(2)))
}, ask = FALSE)
tar_make()
# targets older than 1 week ago
tar_older(Sys.time() - as.difftime(1, units = "weeks"))
# targets older than 1 week from now
tar_older(Sys.time() + as.difftime(1, units = "weeks"))
# Everything is still up to date.
tar_make()
# Invalidate all targets targets older than 1 week from now
# so they run on the next tar_make().
invalidate_these <- tar_older(Sys.time() + as.difftime(1, units = "weeks"))
tar_invalidate(any_of(invalidate_these))
tar_make()
})
}
```
