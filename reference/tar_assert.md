# Assertions

These functions assert the correctness of user inputs and generate
custom error conditions as needed. Useful for writing packages built on
top of `targets`.

## Usage

``` r
tar_assert_chr(x, msg = NULL)

tar_assert_dbl(x, msg = NULL)

tar_assert_df(x, msg = NULL)

tar_assert_equal_lengths(x, msg = NULL)

tar_assert_envir(x, msg = NULL)

tar_assert_expr(x, msg = NULL)

tar_assert_flag(x, choices, msg = NULL)

tar_assert_file(x)

tar_assert_finite(x, msg = NULL)

tar_assert_function(x, msg = NULL)

tar_assert_function_arguments(x, args, msg = NULL)

tar_assert_ge(x, threshold, msg = NULL)

tar_assert_identical(x, y, msg = NULL)

tar_assert_in(x, choices, msg = NULL)

tar_assert_not_dirs(x, msg = NULL)

tar_assert_not_dir(x, msg = NULL)

tar_assert_not_in(x, choices, msg = NULL)

tar_assert_inherits(x, class, msg = NULL)

tar_assert_int(x, msg = NULL)

tar_assert_internet(msg = NULL)

tar_assert_lang(x, msg = NULL)

tar_assert_le(x, threshold, msg = NULL)

tar_assert_list(x, msg = NULL)

tar_assert_lgl(x, msg = NULL)

tar_assert_name(x)

tar_assert_named(x, msg = NULL)

tar_assert_names(x, msg = NULL)

tar_assert_nonempty(x, msg = NULL)

tar_assert_null(x, msg = NULL)

tar_assert_not_expr(x, msg = NULL)

tar_assert_nzchar(x, msg = NULL)

tar_assert_package(package, msg = NULL)

tar_assert_path(path, msg = NULL)

tar_assert_match(x, pattern, msg = NULL)

tar_assert_nonmissing(x, msg = NULL)

tar_assert_positive(x, msg = NULL)

tar_assert_scalar(x, msg = NULL)

tar_assert_store(store)

tar_assert_target(x, msg = NULL)

tar_assert_target_list(x)

tar_assert_true(x, msg = NULL)

tar_assert_unique(x, msg = NULL)

tar_assert_unique_targets(x)
```

## Arguments

- x:

  R object, input to be validated. The kind of object depends on the
  specific assertion function called.

- msg:

  Character of length 1, a message to be printed to the console if `x`
  is invalid.

- choices:

  Character vector of choices of `x` for certain assertions.

- args:

  Character vector of expected function argument names. Order matters.

- threshold:

  Numeric of length 1, lower/upper bound for assertions like
  `tar_assert_le()`/`tar_assert_ge()`.

- y:

  R object, value to compare against `x`.

- class:

  Character vector of expected class names.

- package:

  Character of length 1, name of an R package.

- path:

  Character, file path.

- pattern:

  Character of length 1, a `grep` pattern for certain assertions.

- store:

  Character of length 1, path to the data store of the pipeline.

## See also

Other utilities to extend targets:
[`tar_condition`](https://docs.ropensci.org/targets/reference/tar_condition.md),
[`tar_language`](https://docs.ropensci.org/targets/reference/tar_language.md),
[`tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.md)

## Examples

``` r
tar_assert_chr("123")
try(tar_assert_chr(123))
#> Error : 123 must be a character.
```
