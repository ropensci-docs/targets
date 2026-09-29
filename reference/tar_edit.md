# Open the target script file for editing.

Open the target script file for editing. Requires the `usethis` package.

## Usage

``` r
tar_edit(script = targets::tar_config_get("script"))
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

## Details

The target script file is an R code file that defines the pipeline. The
default path is `_targets.R`, but the default for the current project
can be configured with
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)

## See also

Other scripts:
[`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md),
[`tar_helper()`](https://docs.ropensci.org/targets/reference/tar_helper.md),
[`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md),
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)
