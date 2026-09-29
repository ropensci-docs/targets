# Invoke a `targets` task from inside a `callr` function (without error handling).

Not a user-side function. Do not invoke directly. Exported for internal
purposes only.

## Usage

``` r
tar_callr_inner_try(
  targets_function,
  targets_arguments,
  options,
  envir = NULL,
  parent,
  script,
  store,
  fun,
  pid_parent
)
```

## Arguments

- targets_function:

  A function from `targets` to call.

- targets_arguments:

  Named list of arguments of targets_function.

- options:

  Names of global options to temporarily set in the `callr` process.

- envir:

  Name of the environment to run in. If `NULL`, the environment defaults
  to `tar_option_get("envir")`.

- parent:

  Parent environment of the call to `tar_call_inner()`.

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

- fun:

  Character of length 1, name of the `targets` function being called.

- pid_parent:

  Integer of length 1, process ID of the calling process, e.g. the one
  that called
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).

## Value

The output of a call to a `targets` function that uses `callr` for
reproducibility.

## Examples

``` r
# See the examples of tar_make().
```
