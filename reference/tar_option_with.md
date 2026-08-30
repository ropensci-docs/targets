# Locally set target options.

Locally set target options for the duration of an expression, without
permanently modifying the global state.

## Usage

``` r
tar_option_with(expression, ..., envir_with = parent.frame())
```

## Arguments

- expression:

  An R expression to run with the local option.

- ...:

  Named arguments to
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  to temporarily set for the duration of `expression`.

- envir_with:

  Environment to evaluate `expression`.

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
[`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md)

## Examples

``` r
  tar_option_with(
    tar_target(data, get_data()),
    packages = "dplyr",
    cue = tar_cue(mode = "never")
  )
#> <tar_stem> 
#>   name: data 
#>   description:  
#>   command:
#>     get_data() 
#>   format: rds 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
#>   cue:
#>     seed: TRUE
#>     file: TRUE
#>     iteration: TRUE
#>     repository: TRUE
#>     format: TRUE
#>     depend: TRUE
#>     command: TRUE
#>     mode: never 
#>   packages:
#>     dplyr 
#>   library:
#>     NULL
```
