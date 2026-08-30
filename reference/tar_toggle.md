# Choose code to run based on Target Markdown mode.

Run one piece of code if Target Markdown mode interactive mode is turned
on and another piece of code otherwise.

## Usage

``` r
tar_toggle(interactive, noninteractive)
```

## Arguments

- interactive:

  R code to run if Target Markdown interactive mode is activated.

- noninteractive:

  R code to run if Target Markdown interactive mode is not activated.

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
[`tar_noninteractive()`](https://docs.ropensci.org/targets/reference/tar_noninteractive.md)

## Examples

``` r
tar_toggle(
  message("In interactive mode."),
  message("Not in interactive mode.")
)
#> Not in interactive mode.
```
