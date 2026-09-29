# Check if the target script file exists.

Check if the target script file exists for the current project. The
target script is `_targets.R` by default, but the path can be configured
for the current project using
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md).

## Usage

``` r
tar_exist_script(script = targets::tar_config_get("script"))
```

## Arguments

- script:

  Character of length 1, path to the target script file. Defaults to
  `tar_config_get("script")`, which in turn defaults to `_targets.R`.
  When you set this argument, the value of `tar_config_get("script")` is
  temporarily changed for the current function call. See
  [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md),
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about the target script file and how to set it
  persistently for a project.

## Value

Logical of length 1, whether the current project's metadata exists.

## See also

Other existence:
[`tar_exist_meta()`](https://docs.ropensci.org/targets/reference/tar_exist_meta.md),
[`tar_exist_objects()`](https://docs.ropensci.org/targets/reference/tar_exist_objects.md),
[`tar_exist_process()`](https://docs.ropensci.org/targets/reference/tar_exist_process.md),
[`tar_exist_progress()`](https://docs.ropensci.org/targets/reference/tar_exist_progress.md)

## Examples

``` r
tar_exist_script()
#> [1] FALSE
```
