# Superseded: exponential backoff

Superseded: configure exponential backoff while polling for tasks during
the pipeline.

## Usage

``` r
tar_backoff(min = 0.001, max = 0.1, rate = 1.5)
```

## Arguments

- min:

  Positive numeric of length 1, minimum polling interval in seconds.
  Must be at least `sqrt(.Machine$double.eps)`.

- max:

  Positive numeric of length 1, maximum polling interval in seconds.
  Must be at least `sqrt(.Machine$double.eps)`.

- rate:

  Positive numeric of length 1, greater than or equal to 1.
  Multiplicative rate parameter that allows the exponential backoff
  minimum polling interval to increase from `min` to `max`. Actual
  polling intervals are sampled uniformly from the current minimum to
  `max`.

## Details

This function is superseded and is now only relevant to other superseded
functions
[`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
and
[`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md).
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
uses `crew` in an efficient non-polling way, making exponential backoff
unnecessary.

## Backoff

In high-performance computing it can be expensive to repeatedly poll the
priority queue if no targets are ready to process. The number of seconds
between polls is `runif(1, min, max(max, min * rate ^ index))`, where
`index` is the number of consecutive polls so far that found no targets
ready to skip or run, and `min`, `max`, and `rate` are arguments to
`tar_backoff()`. (If no target is ready, `index` goes up by 1. If a
target is ready, `index` resets to 0. For more information on
exponential, backoff, visit
<https://en.wikipedia.org/wiki/Exponential_backoff>). Raising `min` or
`max` is kinder to the CPU etc. but may incur delays in some instances.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md),
[`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md),
[`tar_definition()`](https://docs.ropensci.org/targets/reference/tar_definition.md),
[`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md),
[`tar_envir()`](https://docs.ropensci.org/targets/reference/tar_envir.md),
[`tar_format_get()`](https://docs.ropensci.org/targets/reference/tar_format_get.md),
[`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.md),
[`tar_name()`](https://docs.ropensci.org/targets/reference/tar_name.md),
[`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md),
[`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md),
[`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md),
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md),
[`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_option_set(backoff = tar_backoff(min = 0.001, max = 0.1, rate = 1.5))
})
}
```
