# Deprecated: `callr` arguments.

Deprecated on 2022-08-05 (`targets` version 0.13.1). Please use
[`tar_callr_args_default()`](https://docs.ropensci.org/targets/reference/tar_callr_args_default.md)
instead.

## Usage

``` r
callr_args_default(callr_function, reporter = NULL)
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
