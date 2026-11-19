# Deprecated: get the seed of the current target.

Deprecated on 2023-10-12 (`targets` version 1.3.2.9001). Use
[`tar_seed_get()`](https://docs.ropensci.org/targets/reference/tar_seed_get.md)
instead.

## Usage

``` r
tar_seed(default = 1L)
```

## Arguments

- default:

  Integer, value to return if
  [`tar_seed_get()`](https://docs.ropensci.org/targets/reference/tar_seed_get.md)
  is called on its own outside a `targets` pipeline. Having a default
  lets users run things without
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  which helps peel back layers of code and troubleshoot bugs.

## Value

Integer of length 1. If invoked inside a `targets` pipeline, the return
value is the seed of the target currently running, which is a
deterministic function of the target name. Otherwise, the return value
is `default`.

## Examples

``` r
tar_seed_get()
#> [1] 1
tar_seed_get(default = 123L)
#> [1] 123
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(tar_target(returns_seed, tar_seed_get()), ask = FALSE)
tar_make()
tar_read(returns_seed)
})
}
```
