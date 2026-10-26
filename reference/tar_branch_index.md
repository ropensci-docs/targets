# Integer branch indexes

Get the integer indexes of individual branch names within their
corresponding dynamic branching targets.

## Usage

``` r
tar_branch_index(names, store = targets::tar_config_get("store"))
```

## Arguments

- names:

  Character vector of branch names.

- store:

  Character of length 1, path to the `targets` data store. Defaults to
  `tar_config_get("store")`, which in turn defaults to `_targets/`. When
  you set this argument, the value of `tar_config_get("store")` is
  temporarily changed for the current function call. See
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about how to set the data store path persistently for a
  project.

## Value

A named integer vector of branch indexes.

## See also

Other branching:
[`tar_branch_names()`](https://docs.ropensci.org/targets/reference/tar_branch_names.md),
[`tar_branches()`](https://docs.ropensci.org/targets/reference/tar_branches.md),
[`tar_pattern()`](https://docs.ropensci.org/targets/reference/tar_pattern.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(x, seq_len(4)),
    tar_target(y, 2 * x, pattern = map(x)),
    tar_target(z, y, pattern = map(y))
  )
}, ask = FALSE)
tar_make()
names <- c(
  tar_meta(y, children)$children[[1]][c(2, 3)],
  tar_meta(z, children)$children[[1]][2]
)
names
tar_branch_index(names) # c(2, 3, 2)
})
}
```
