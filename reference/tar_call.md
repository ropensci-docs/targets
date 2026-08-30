# Identify the called `targets` function.

Get the name of the currently running `targets` interface function.
Returns `NULL` if not invoked inside a target or `_targets.R` (i.e. if
not directly invoked by
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
etc.).

## Usage

``` r
tar_call()
```

## Value

Character of length 1, name of the currently running `targets` interface
function. For example, suppose you have a call to `tar_call()` inside a
target or `_targets.R`. Then if you run
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
`tar_call()` will return `"tar_make"`.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
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
tar_call() # NULL
tar_script({
  library(targets)
  library(tarchetypes)
  message("called function: ", tar_call())
  tar_target(x, tar_call())
})
tar_manifest() # prints "called function: tar_manifest"
tar_make() # prints "called function: tar_make"
tar_read(x) # "tar_make"
})
}
```
