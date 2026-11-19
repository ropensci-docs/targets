# Default `callr` arguments.

Default `callr` arguments for the `callr_arguments` argument of
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
and related functions.

## Usage

``` r
tar_callr_args_default(callr_function, reporter = NULL)
```

## Arguments

- callr_function:

  A function from the `callr` package that starts an external R process.

- reporter:

  Character of length 1, choice of reporter for
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  or a related function.

## Value

A list of arguments to `callr_function`.

## Details

Not a user-side function. Do not invoke directly. Exported for internal
purposes only.

## Examples

``` r
tar_callr_args_default(callr::r)
#> $spinner
#> [1] FALSE
#> 
#> $stderr
#> [1] "2>&1"
#> 
```
