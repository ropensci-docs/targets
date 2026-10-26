# Run if Target Markdown interactive mode is not on.

In Target Markdown, run the enclosed code only if interactive mode is
not activated. Otherwise, do not run the code.

## Usage

``` r
tar_noninteractive(code)
```

## Arguments

- code:

  R code to run if Target Markdown interactive mode is not turned on.

## Value

If Target Markdown interactive mode is not turned on, the function
returns the result of running the code. Otherwise, the function
invisibly returns `NULL`.

## Details

Visit \<books.ropensci.org/targets/literate-programming.html\> to learn
about Target Markdown and interactive mode.

## See also

Other Target Markdown:
[`tar_engine_knitr()`](https://docs.ropensci.org/targets/reference/tar_engine_knitr.md),
[`tar_interactive()`](https://docs.ropensci.org/targets/reference/tar_interactive.md),
[`tar_toggle()`](https://docs.ropensci.org/targets/reference/tar_toggle.md)

## Examples

``` r
tar_noninteractive(message("Not in interactive mode."))
#> Not in interactive mode.
```
