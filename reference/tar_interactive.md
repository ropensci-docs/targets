# Run if Target Markdown interactive mode is on.

In Target Markdown, run the enclosed code only if interactive mode is
activated. Otherwise, do not run the code.

## Usage

``` r
tar_interactive(code)
```

## Arguments

- code:

  R code to run if Target Markdown interactive mode is turned on.

## Value

If Target Markdown interactive mode is turned on, the function returns
the result of running the code. Otherwise, the function invisibly
returns `NULL`.

## Details

Visit \<books.ropensci.org/targets/literate-programming.html\> to learn
about Target Markdown and interactive mode.

## See also

Other Target Markdown:
[`tar_engine_knitr()`](https://docs.ropensci.org/targets/reference/tar_engine_knitr.md),
[`tar_noninteractive()`](https://docs.ropensci.org/targets/reference/tar_noninteractive.md),
[`tar_toggle()`](https://docs.ropensci.org/targets/reference/tar_toggle.md)

## Examples

``` r
tar_interactive(message("In interactive mode."))
```
