# Reproducible example of `targets` with `reprex`

Create a reproducible example of a `targets` pipeline with the `reprex`
package.

## Usage

``` r
tar_reprex(pipeline = tar_target(example_target, 1), run = tar_make(), ...)
```

## Arguments

- pipeline:

  R code for the target script file `_targets.R`.
  [`library(targets)`](https://docs.ropensci.org/targets/) is
  automatically written at the top.

- run:

  R code to inspect and run the pipeline.

- ...:

  Named arguments passed to
  [`reprex::reprex()`](https://reprex.tidyverse.org/reference/reprex.html).

## Value

A character vector of rendered the reprex, invisibly.

## Details

The best way to get help with an issue is to create a reproducible
example of the problem and post it to
<https://github.com/ropensci/targets/discussions> `tar_reprex()`
facilitates this process. It is like
`reprex::reprex({targets::tar_script(...); tar_make()})`, but more
convenient.

## See also

Other help:
[`targets-package`](https://docs.ropensci.org/targets/reference/targets-package.md),
[`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md),
[`use_targets_rmd()`](https://docs.ropensci.org/targets/reference/use_targets_rmd.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_reprex(
  pipeline = {
    list(
      tar_target(data, data.frame(x = sample.int(1e3))),
      tar_target(summary, mean(data$x, na.rm = TRUE))
    )
  },
  run = {
    tar_visnetwork()
    tar_make()
  }
)
}
```
