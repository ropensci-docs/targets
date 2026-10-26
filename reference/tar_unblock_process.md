# Unblock the pipeline process

`targets` tries to avoid running two concurrent instances of
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
on the same pipeline writing to the same data store. Sometimes it
generates false positives (meaning
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
throws this error even though there is only one instance of the pipeline
running.) If there is a false positive, `tar_unblock_process()` gets the
pipeline unstuck by removing the `_targets/meta/process` file. This
allows the next call to
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
to resume.

## Usage

``` r
tar_unblock_process(store = targets::tar_config_get("store"))
```

## Arguments

- store:

  Character string, path to the data store (usually `"_targets"`).

## Value

`NULL` (invisibly). Called for its side effects.

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
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md)
