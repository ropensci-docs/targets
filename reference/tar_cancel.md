# Cancel a target mid-execution under a custom condition.

Cancel a target while its command is running if a condition is met.

## Usage

``` r
tar_cancel(condition = TRUE)
```

## Arguments

- condition:

  Logical of length 1, whether to cancel the target.

## Details

Must be invoked by the target itself. `tar_cancel()` cannot interrupt a
target from another process.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
[`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md),
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
tar_script(tar_target(x, tar_cancel(1 > 0)))
tar_make() # Should cancel target x.
})
}
```
