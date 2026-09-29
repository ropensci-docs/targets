# Use targets with Target Markdown.

Create an example Target Markdown report to get started with `targets`.

## Usage

``` r
use_targets_rmd(path = "_targets.Rmd", open = interactive())
```

## Arguments

- path:

  Character of length 1, output path of the Target Markdown report
  relative to the current active project.

- open:

  Logical, whether to open the file for editing in the RStudio IDE.

## Value

`NULL` (invisibly).

## See also

Other help:
[`tar_reprex()`](https://docs.ropensci.org/targets/reference/tar_reprex.md),
[`targets-package`](https://docs.ropensci.org/targets/reference/targets-package.md),
[`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
use_targets(open = FALSE)
})
}
```
