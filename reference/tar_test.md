# Test code in a temporary directory.

Runs a `test_that()` unit test inside a temporary directory to avoid
writing to the user's file space. This helps ensure compliance with CRAN
policies. Also isolates
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
options and environment variables specific to `targets` and skips the
test on Solaris. Useful for writing tests for
[targetopia](https://wlandau.github.io/targetopia/) packages (extensions
to `targets` tailored to specific use cases).

## Usage

``` r
tar_test(label, code)
```

## Arguments

- label:

  Character of length 1, label for the test.

- code:

  User-defined code for the test.

## Value

`NULL` (invisibly).

## See also

Other utilities to extend targets:
[`tar_assert`](https://docs.ropensci.org/targets/reference/tar_assert.md),
[`tar_condition`](https://docs.ropensci.org/targets/reference/tar_condition.md),
[`tar_language`](https://docs.ropensci.org/targets/reference/tar_language.md)

## Examples

``` r
tar_test("example test", {
  testing_variable_cafecfcb <- "only defined inside tar_test()"
  file.create("only_exists_in_tar_test")
})
#> ── Skip: example test ──────────────────────────────────────────────────────────
#> Reason: empty test
exists("testing_variable_cafecfcb")
#> [1] FALSE
file.exists("only_exists_in_tar_test")
#> [1] FALSE
```
