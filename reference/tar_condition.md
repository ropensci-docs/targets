# Conditions

These functions throw custom `targets`-specific error conditions. Useful
for error handling in packages built on top of `targets`.

## Usage

``` r
tar_message_run(...)

tar_throw_file(...)

tar_throw_run(..., class = character(0))

tar_throw_validate(...)

tar_warn_deprecate(...)

tar_warn_run(...)

tar_warn_validate(...)

tar_message_validate(...)

tar_print(...)

tar_error(message, class)

tar_warning(message, class)

tar_message(message, class)
```

## Arguments

- ...:

  zero or more objects which can be coerced to character (and which are
  pasted together with no separator) or a single condition object.

- class:

  Character vector of S3 classes of the message.

- message:

  Character of length 1, text of the message.

## See also

Other utilities to extend targets:
[`tar_assert`](https://docs.ropensci.org/targets/reference/tar_assert.md),
[`tar_language`](https://docs.ropensci.org/targets/reference/tar_language.md),
[`tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.md)

## Examples

``` r
try(tar_throw_validate("something is not valid"))
#> Error : something is not valid
```
