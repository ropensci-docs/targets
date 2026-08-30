# Get a target option.

Get a target option. These options include default arguments to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
such as packages, storage format, iteration type, and cue. Needs to be
called before any calls to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
in order to take effect.

## Usage

``` r
tar_option_get(name = NULL, option = NULL)
```

## Arguments

- name:

  Character of length 1, name of an option to get. Must be one of the
  argument names of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).

- option:

  Deprecated, use the `name` argument instead.

## Value

Value of a target option.

## Details

This function goes well with
[`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md)
when it comes to defining external interfaces on top of the `targets`
package to create pipelines.

## See also

Other configuration:
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
[`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md),
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md),
[`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md),
[`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md),
[`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md),
[`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md),
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
[`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md),
[`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)

## Examples

``` r
tar_option_get("format") # default format before we set anything
#> [1] "rds"
tar_target(x, 1)$settings$format
#> [1] "rds"
tar_option_set(format = "fst_tbl") # new default format
tar_option_get("format")
#> [1] "fst_tbl"
tar_target(x, 1)$settings$format
#> [1] "fst_tbl"
tar_option_reset() # reset the format
tar_target(x, 1)$settings$format
#> [1] "rds"
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(cue = tar_cue(mode = "always")) # All targets always run.
  list(tar_target(x, 1), tar_target(y, 2))
})
tar_make()
tar_make()
})
}
```
