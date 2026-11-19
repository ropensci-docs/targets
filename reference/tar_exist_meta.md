# Check if target metadata exists.

Check if the target metadata file `_targets/meta/meta` exists for the
current project.

## Usage

``` r
tar_exist_meta(store = targets::tar_config_get("store"))
```

## Arguments

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

Logical of length 1, whether the current project's metadata exists.

## Details

To learn more about data storage in `targets`, visit
<https://books.ropensci.org/targets/data.html>.

## See also

Other existence:
[`tar_exist_objects()`](https://docs.ropensci.org/targets/reference/tar_exist_objects.md),
[`tar_exist_process()`](https://docs.ropensci.org/targets/reference/tar_exist_process.md),
[`tar_exist_progress()`](https://docs.ropensci.org/targets/reference/tar_exist_progress.md),
[`tar_exist_script()`](https://docs.ropensci.org/targets/reference/tar_exist_script.md)

## Examples

``` r
tar_exist_meta()
#> [1] FALSE
```
