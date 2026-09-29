# Branch names

Get the branch names of a dynamic branching target using numeric
indexes. `tar_branch_names()` expects an unevaluated symbol for the
`name` argument, whereas `tar_branch_names_raw()` expects a character
string for `name`.

## Usage

``` r
tar_branch_names(name, index, store = targets::tar_config_get("store"))

tar_branch_names_raw(name, index, store = targets::tar_config_get("store"))
```

## Arguments

- name:

  Name of the dynamic branching target. `tar_branch_names()` expects an
  unevaluated symbol for the `name` argument, whereas
  `tar_branch_names_raw()` expects a character string for `name`.

- index:

  Integer vector of branch indexes.

- store:

  Character string, directory path to the `targets` data store of the
  pipeline.

## Value

A character vector of branch names.

## See also

Other branching:
[`tar_branch_index()`](https://docs.ropensci.org/targets/reference/tar_branch_index.md),
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
tar_branch_names(z, c(2, 3))
})
}
```
