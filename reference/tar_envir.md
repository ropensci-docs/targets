# For developers only: get the environment of the current target.

For developers only: get the environment where a target runs its
command. Designed to be called while the target is running. The
environment inherits from `tar_option_get("envir")`.

## Usage

``` r
tar_envir(default = parent.frame())
```

## Arguments

- default:

  Environment, value to return if `tar_envir()` is called on its own
  outside a `targets` pipeline. Having a default lets users run things
  without
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  which helps peel back layers of code and troubleshoot bugs.

## Value

If called from a running target, `tar_envir()` returns the environment
where the target runs its command. If called outside a pipeline, the
return value is whatever the user supplies to `default` (which defaults
to [`parent.frame()`](https://rdrr.io/r/base/sys.parent.html)).

## Details

Most users should not use `tar_envir()` because accidental modifications
to `parent.env(tar_envir())` could break the pipeline. `tar_envir()`
only exists in order to support third-party interface packages, and even
then the returned environment is not modified.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
[`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md),
[`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md),
[`tar_definition()`](https://docs.ropensci.org/targets/reference/tar_definition.md),
[`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md),
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
tar_envir()
#> <environment: 0x560cca85c170>
tar_envir(default = new.env(parent = emptyenv()))
#> <environment: 0x560cca80f008>
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(tar_target(x, tar_envir(default = parent.frame())))
tar_make(x)
tar_read(x)
})
}
```
