# Language

These functions help with metaprogramming in packages built on top of
`targets`.

## Usage

``` r
tar_deparse_language(expr)

tar_deparse_safe(expr, collapse = "\n", backtick = TRUE)

tar_tidy_eval(expr, envir, tidy_eval)

tar_tidyselect_eval(names_quosure, choices, strict = TRUE)
```

## Arguments

- expr:

  A language object to modify or deparse.

- collapse:

  Character of length 1, delimiter in deparsing.

- backtick:

  logical indicating whether symbolic names should be enclosed in
  backticks if they do not follow the standard syntax.

- envir:

  An environment to find objects for tidy evaluation.

- tidy_eval:

  Logical of length 1, whether to apply tidy evaluation.

- names_quosure:

  An `rlang` quosure with `tidyselect` expressions.

- choices:

  A character vector of choices for character elements returned by tidy
  evaluation.

- strict:

  If `TRUE`, out-of-bounds errors are thrown if `expr` attempts to
  select or rename a variable that doesn't exist. If `FALSE`, failed
  selections or renamings are ignored.

## Details

- `tar_deparse_language()` is a wrapper around `tar_deparse_safe()`
  which leaves character vectors and `NULL` objects alone, which helps
  with subsequent user input validation.

- `tar_deparse_safe()` is a wrapper around
  [`base::deparse()`](https://rdrr.io/r/base/deparse.html) with a custom
  set of fast default settings and guardrails to ensure the output
  always has length 1.

- `tar_tidy_eval()` applies tidy evaluation to a language object and
  returns another language object.

- `tar_tidyselect_eval()` applies `tidyselect` selection with some
  special guardrails around `NULL` inputs.

## See also

Other utilities to extend targets:
[`tar_assert`](https://docs.ropensci.org/targets/reference/tar_assert.md),
[`tar_condition`](https://docs.ropensci.org/targets/reference/tar_condition.md),
[`tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.md)

## Examples

``` r
tar_deparse_language(quote(run_model()))
#> [1] "run_model()"
```
