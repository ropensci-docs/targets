# targets: Dynamic Function-Oriented Make-Like Declarative Pipelines for R

A pipeline toolkit for Statistics and data science in R, the `targets`
package brings function-oriented programming to Make-like declarative
pipelines. `targets` orchestrates a pipeline as a graph of dependencies,
skips steps that are already up to date, runs the necessary computations
with optional parallel workers, abstracts files as R objects, and
provides tangible evidence that the results are reproducible given the
underlying code and data. The methodology in this package borrows from
GNU Make (2015, ISBN:978-9881443519) and `drake` (2018,
[doi:10.21105/joss.00550](https://doi.org/10.21105/joss.00550) ).

## See also

Other help:
[`tar_reprex()`](https://docs.ropensci.org/targets/reference/tar_reprex.md),
[`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md),
[`use_targets_rmd()`](https://docs.ropensci.org/targets/reference/use_targets_rmd.md)
