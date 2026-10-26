# Identify the file path where the current target will be stored.

Identify the file path where the current target will be stored locally
on disk. Designed to be called inside the command of a target currently
running in a pipeline. See the "Value" section for specifics because the
return value depends on the `format` and `repository` settings.

## Usage

``` r
tar_path_target(
  name = NULL,
  default = NA_character_,
  create_dir = FALSE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- name:

  Symbol, name of a target. If `NULL`, `tar_path_target()` returns the
  path of the target currently running in a pipeline.

- default:

  Character, value to return if `tar_path_target()` is called on its own
  outside a `targets` pipeline. Having a default lets users run things
  without
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  which helps peel back layers of code and troubleshoot bugs.

- create_dir:

  Logical of length 1, whether to create `dirname(tar_path_target())` in
  `tar_path_target()` itself. This is useful if you are writing to
  `tar_path_target()` from inside a `storage = "none"` target and need
  the parent directory of the file to exist.

- store:

  Character of length 1, path to the data store if `tar_path_target()`
  is called outside a running pipeline. If `tar_path_target()` is called
  inside a running pipeline, this argument is ignored and actual the
  path to the running pipeline's data store is used instead.

## Value

Character string, the file path where the target currently running will
be stored. This path is not always known or available in advance, it
depends on the `format` and `repository` settings.

If `format` is not `"file"` and `repository` is `"local"` (default) then
`tar_path_target()` returns `"STORE/objects/YOUR_TARGET"`, where `STORE`
is the value of `tar_config_get("store")` and `YOUR_TARGET` is the name
of your target.

Otherwise, if `format` is `"file"`, then the return value is
`NA_character_` because `targets` does not control where those files are
stored.

Otherwise, if `format` is not `"file"` and `repository` is not
`"local"`, then `tar_path_target()` returns the temporary staging path
where the data is stored before it is uploaded to a remote repository.
Remote repositories vary so widely that the eventual final location
cannot always be known, but all remote repositories use staging files
that `targets` knows about.

If not called from inside a running target,
`tar_path_target(name = your_target)` just returns
`"STORE/objects/your_target"`.

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
[`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md),
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
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
