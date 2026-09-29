# Directory path to the support scripts of the current target script

Identify the directory path to the support scripts of the current target
script of the pipeline currently running.

## Usage

``` r
tar_path_script_support()
```

## Value

Character, directory path to the target script of the pipeline currently
running. If called outside of the pipeline currently running,
[`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md)
returns `tar_config_get("script")`.

## Details

A target script (default: `_targets.R`) comes with support scripts if it
is written by Target Markdown. These support scripts usually live in a
folder called `_targets_r/`, but the path may vary from case to case.
The `tar_path_scipt_support()` returns the path to the folder with the
support scripts.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
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
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md),
[`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)

## Examples

``` r
tar_path_script_support()
#> [1] "_targets_r"
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
script <- tempfile()
tar_script(
  tar_target(x, tar_path_script_support()),
  script = script,
  ask = FALSE
)
tar_make(script = script)
tar_read(x)
})
}
```
