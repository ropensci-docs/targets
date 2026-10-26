# Internal function to run a target on a worker.

For internal purposes only. Not a user-side function.

## Usage

``` r
target_run_worker(target, envir, path_store, fun, options, envvars)
```

## Arguments

- target:

  A target definition object.

- envir:

  An environment or the string `"globalenv"`.

- path_store:

  Character of length 1, path to the data store.

- fun:

  Character of length 1, name of the user-side function called to run
  the pipeline.

- options:

  List, exported from an object of class `"tar_options"`.

- envvars:

  Data frame of `targets`-specific environment variables from
  [`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md).
