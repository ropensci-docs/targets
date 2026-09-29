# Check if local output data exists for one or more targets.

Check if output target data exists in either `_targets/objects/` or the
cloud for one or more targets.

## Usage

``` r
tar_exist_objects(
  names,
  cloud = TRUE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Character vector of target names. Not `tidyselect`-compatible.

- cloud:

  Logical of length 1, whether to include cloud targets in the output
  (e.g. `tar_target(..., repository = "aws")`).

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

Logical of length `length(names)`, whether each given target has an
existing file in either `_targets/objects/` or the cloud.

## Details

If a target has no metadata or if the `repository` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
was set to `"local"`, then the `_targets/objects/` folder is checked.
Otherwise, if there is metadata and `repsitory` is not `"local"`, then
`tar_exist_objects()` checks the cloud repository selected.

## See also

Other existence:
[`tar_exist_meta()`](https://docs.ropensci.org/targets/reference/tar_exist_meta.md),
[`tar_exist_process()`](https://docs.ropensci.org/targets/reference/tar_exist_process.md),
[`tar_exist_progress()`](https://docs.ropensci.org/targets/reference/tar_exist_progress.md),
[`tar_exist_script()`](https://docs.ropensci.org/targets/reference/tar_exist_script.md)

## Examples

``` r
tar_exist_objects(c("target1", "target2"))
#> [1] FALSE FALSE
```
