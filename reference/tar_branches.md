# Reconstruct the branch names and the names of their dependencies.

Given a branching pattern, use available metadata to reconstruct branch
names and the names of each branch's dependencies. The metadata of each
target must already exist and be consistent with the metadata of the
other targets involved.

## Usage

``` r
tar_branches(
  name,
  pattern = NULL,
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- name:

  Symbol, name of the target.

- pattern:

  Language to define branching for a target (just like in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md))
  or `NULL` to get the pattern from the `targets` pipeline script
  specified in the `script` argument (default: `_targets.R`).

- script:

  Character of length 1, path to the target script file. Defaults to
  `tar_config_get("script")`, which in turn defaults to `_targets.R`.
  When you set this argument, the value of `tar_config_get("script")` is
  temporarily changed for the current function call. See
  [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md),
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about the target script file and how to set it
  persistently for a project.

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

A `tibble` with one row per branch and one column for each target
(including the branched-over targets and the target with the pattern.)

## Details

The results from this function can help you retroactively figure out
correspondences between upstream branches and downstream branches.
However, it does not always correctly predict what the names of the
branches will be after the next run of the pipeline. Dynamic branching
happens while the pipeline is running, so we cannot always know what the
names of the branches will be in advance (or even how many there will
be).

## See also

Other branching:
[`tar_branch_index()`](https://docs.ropensci.org/targets/reference/tar_branch_index.md),
[`tar_branch_names()`](https://docs.ropensci.org/targets/reference/tar_branch_names.md),
[`tar_pattern()`](https://docs.ropensci.org/targets/reference/tar_pattern.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(x, seq_len(2)),
    tar_target(y, head(letters, 2)),
    tar_target(z, head(LETTERS, 2)),
    tar_target(dynamic, c(x, y, z), pattern = cross(z, map(x, y)))
  )
}, ask = FALSE)
tar_make()
tar_branches(dynamic)
tar_branches(dynamic, pattern = cross(z, map(x, y)))
})
}
```
