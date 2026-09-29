# Unset one or more target options.

Unset one or more target options you previously chose with
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).
These options are mostly configurable default arguments to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
and
[`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## Usage

``` r
tar_option_unset(options)
```

## Arguments

- options:

  A character vector of options to reset. Must all be elements of
  `names(formals(tar_option_set))`

## Value

`NULL` (invisibly).

## See also

Other configuration:
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
[`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md),
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md),
[`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md),
[`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md),
[`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md),
[`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
[`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md),
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
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
tar_option_unset("format") # Unsets the format option.
tar_target(x, 1)$settings$format
#> [1] "rds"
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(cue = tar_cue(mode = "always"))
  tar_option_unset("cue") # Unsets the cue option.
  list(tar_target(x, 1), tar_target(y, 2))
})
tar_make()
tar_make()
})
}
```
