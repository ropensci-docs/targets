# Get a target's traceback

Return the saved traceback of a target. Assumes the target errored out
in a previous run of the pipeline with workspaces enabled for that
target. See
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
for details.

## Usage

``` r
tar_traceback(
  name,
  envir = NULL,
  packages = NULL,
  source = NULL,
  characters = NULL,
  store = targets::tar_config_get("store")
)
```

## Arguments

- name:

  Symbol, name of the target whose workspace to read.

- envir:

  Deprecated in `targets` \> 0.3.1 (2021-03-28).

- packages:

  Logical, whether to load the required packages of the target.

- source:

  Logical, whether to run the target script file (default: `_targets.R`)
  to load user-defined global object dependencies into `envir`. If
  `TRUE`, then `envir` should either be the global environment or
  inherit from the global environment.

- characters:

  Deprecated in `targets` 1.4.0 (2023-12-06).

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

Character vector, the traceback of a failed target if it exists.

## See also

Other debug:
[`tar_load_globals()`](https://docs.ropensci.org/targets/reference/tar_load_globals.md),
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md),
[`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md),
[`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tmp <- sample(1)
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(workspace_on_error = TRUE)
  list(
    tar_target(x, "loaded"),
    tar_target(y, stop(x))
  )
}, ask = FALSE)
try(tar_make())
tar_traceback(y, characters = 60)
})
}
```
