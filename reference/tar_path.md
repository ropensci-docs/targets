# Deprecated: identify the file path where a target will be stored.

Deprecated: identify the file path where a target will be stored after
the target finishes running in the pipeline.

## Usage

``` r
tar_path(
  name = NULL,
  default = NA_character_,
  create_dir = FALSE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- name:

  Symbol, name of a target. If `NULL`, `tar_path()` returns the path of
  the target currently running in a pipeline.

- default:

  Character, value to return if `tar_path()` is called on its own
  outside a `targets` pipeline. Having a default lets users run things
  without
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  which helps peel back layers of code and troubleshoot bugs.

- create_dir:

  Logical of length 1, whether to create `dirname(tar_path())` in
  `tar_path()` itself. This is useful if you are writing to `tar_path()`
  from inside a `storage = "none"` target and need the parent directory
  of the file to exist.

- store:

  Character of length 1, path to the data store if `tar_path()` is
  called outside a running pipeline. If `tar_path()` is called inside a
  running pipeline, this argument is ignored and actual the path to the
  running pipeline's data store is used instead.

## Value

Character, file path of the return value of the target. If not called
from inside a running target, `tar_path(name = your_target)` just
returns `_targets/objects/your_target`, the file path where
`your_target` will be saved unless `format` is equal to `"file"` or any
of the supported cloud-based storage formats.

For non-cloud storage formats, if you call `tar_path()` with no
arguments while target `x` is running, the `name` argument defaults to
the name of the running target, so `tar_path()` returns
`_targets/objects/x`.

For cloud-backed formats, `tar_path()` returns the path to the staging
file in `_targets/scratch/`. That way, even if you select a cloud
repository (e.g.
`tar_target(..., repository = "aws", storage = "none")`) then you can
still manually write to `tar_path(create_dir = TRUE)` and the `targets`
package will automatically hash it and upload it to the AWS S3 bucket.
This does not apply to `format = "file"`, where you would never need
`storage = "none"` anyway.

## Details

`tar_path()` was deprecated on 2022-10-11 (version 0.13.5.9000). Use
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md)
instead.

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
[`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md),
[`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md),
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md),
[`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)

## Examples

``` r
tar_path_target()
#> [1] NA
tar_path_target(your_target)
#> [1] "_targets/objects/your_target"
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(tar_target(returns_path, tar_path_target()), ask = FALSE)
tar_make()
tar_read(returns_path)
})
}
```
