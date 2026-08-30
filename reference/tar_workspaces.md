# List locally saved target workspaces.

List target workspaces currently saved to `_targets/workspaces/`
locally. Does not include workspaces saved to the cloud. See
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
and
[`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md)
for more information.

## Usage

``` r
tar_workspaces(names = NULL, store = targets::tar_config_get("store"))
```

## Arguments

- names:

  Optional `tidyselect` selector to return a tactical subset of
  workspace names. If `NULL`, all names are selected. The object
  supplied to `names` should be `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

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

Character vector of available workspaces to load with
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md).

## See also

Other debug:
[`tar_load_globals()`](https://docs.ropensci.org/targets/reference/tar_load_globals.md),
[`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md),
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md),
[`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(workspace_on_error = TRUE)
  list(
    tar_target(x, "value"),
    tar_target(y, x)
  )
}, ask = FALSE)
tar_make()
tar_workspaces()
tar_workspaces(contains("x"))
})
}
```
