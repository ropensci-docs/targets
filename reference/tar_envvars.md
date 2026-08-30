# Show `targets` environment variables.

Show all the special environment variables available for customizing
`targets`.

## Usage

``` r
tar_envvars(unset = "")
```

## Arguments

- unset:

  Character of length 1, value to return for any environment variable
  that is not set.

## Value

A data frame with one row per environment variable and columns with the
name and current value of each. An unset environment variable will have
a value of `""` by default. (Customize with the `unset` argument).

## Details

You can customize the behavior of `targets` with special environment
variables. The sections in this help file describe each environment
variable, and the `tar_envvars()` function lists their current values.

If you modify environment variables, please set them in project-level
`.Renviron` file so you do not lose your configuration when you restart
your R session. Modify the project-level `.Renviron` file with
`usethis::edit_r_environ(scope = "project")`. Restart your R session
after you are done editing.

For targets that run on parallel workers created by
[`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
or
[`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
only the environment variables listed by `tar_envvars()` are
specifically exported to the targets. For all other environment
variables, you will have to set the values manually, e.g. a
project-level `.Renviron` file (for workers that have access to the
local file system).

## TAR_ACTIVE

The `TAR_ACTIVE` environment variable is automatically `"true"` if the
current process is part of a `targets` pipeline and unset otherwise.

## TAR_ASK

The `TAR_ASK` environment variable accepts values `"true"` and
`"false"`. If `TAR_ASK` is not set, or if it is set to `"true"`, then
`targets` asks permission in a menu before overwriting certain files,
such as the target script file (default: `_targets.R`) in
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md).
If `TAR_ASK` is `"false"`, then `targets` overwrites the old files with
the new ones without asking. Once you are comfortable with
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md),
[`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md),
and similar functions, you can safely set `TAR_ASK` to `"false"` in
either a project-level or user-level `.Renviron` file.

## TAR_CONFIG

The `TAR_CONFIG` environment variable controls the file path to the
optional YAML configuration file with project settings. See the help
file of
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
for details.

## TAR_PROJECT

The `TAR_PROJECT` environment variable sets the name of project to set
and get settings when working with the YAML configuration file. See the
help file of
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
for details.

## TAR_WARN

The `TAR_WARN` environment variable accepts values `"true"` and
`"false"`. If `TAR_WARN` is not set, or if it is set to `"true"`, then
`targets` throws warnings in certain edge cases, such as target/global
name conflicts and dangerous use of `devtools::load_all()`. If
`TAR_WARN` is `"false"`, then `targets` does not throw warnings in these
cases. These warnings can detect potentially serious issues with your
pipeline, so please do not set `TAR_WARN` unless your use case
absolutely requires it.

## See also

Other configuration:
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
[`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md),
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md),
[`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md),
[`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md),
[`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
[`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md),
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
[`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md),
[`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)

## Examples

``` r
tar_envvars()
#> # A tibble: 5 × 2
#>   name        value
#>   <chr>       <chr>
#> 1 TAR_ACTIVE  ""   
#> 2 TAR_ASK     ""   
#> 3 TAR_CONFIG  ""   
#> 4 TAR_PROJECT ""   
#> 5 TAR_WARN    ""   
```
