# Execute code in a temporary directory.

Not a user-side function. Just for CRAN.

## Usage

``` r
tar_dir(code)
```

## Arguments

- code:

  User-defined code.

## Value

Return value of the user-defined code.

## Details

Runs code inside a new
[`tempfile()`](https://rdrr.io/r/base/tempfile.html) directory in order
to avoid writing to the user's file space. Used in examples and tests in
order to comply with CRAN policies.

## Examples

``` r
tar_dir(file.create("only_exists_in_tar_dir"))
#> [1] TRUE
file.exists("only_exists_in_tar_dir")
#> [1] FALSE
```
