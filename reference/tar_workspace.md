# Load a locally saved workspace and seed for debugging.

Load the packages, environment, and random number generator seed of a
target.

## Usage

``` r
tar_workspace(
  name,
  envir = parent.frame(),
  packages = TRUE,
  source = TRUE,
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- name:

  Symbol, name of the target whose workspace to read.

- envir:

  Environment in which to put the objects.

- packages:

  Logical, whether to load the required packages of the target.

- source:

  Logical, whether to run `_targets.R` to load user-defined global
  object dependencies into `envir`. If `TRUE`, then `envir` should
  either be the global environment or inherit from the global
  environment.

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

This function returns `NULL`, but it does load the target's required
packages, as well as multiple objects into the environment (`envir`
argument) in order to replicate the workspace where the error happened.
These objects include the global objects at the time
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
was called and the dependency targets. The random number generator seed
for the target is also assigned with
[`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md).

## Details

If you activate workspaces through the `workspaces` argument of
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
then under the circumstances you specify, `targets` will save a special
workspace file to a location in in `_targets/workspaces/`. The workspace
file is a compact reference that allows `tar_workspace()` to load the
target's dependencies and random number generator seed as long as the
data objects are still in the data store (usually files in
`_targets/objects/`). When you are done debugging, you can remove the
workspace files using `tar_destroy(destroy = "workspaces")`.

If `tar_option_get("repository_meta")` is `"aws"` or `"gcp"`, then
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
uploads workspaces to the bucket and prefix provided. Download one of
these workspaces with
[`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md).
Downloaded workspaces can be loaded the usual way with
`tar_workspace()`, and you should see them in character vector returned
by
[`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md).

## See also

Other debug:
[`tar_load_globals()`](https://docs.ropensci.org/targets/reference/tar_load_globals.md),
[`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md),
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
  list(
    tar_target(x, "loaded"),
    tar_target(y, stop(x))
  )
}, ask = FALSE)
# The following code throws an error for demonstration purposes.
try(tar_make())
exists("x") # Should be FALSE.
tail(.Random.seed) # for comparison to the RNG state after tar_workspace(y)
tar_workspace(y)
exists("x") # Should be TRUE.
print(x) # "loaded"
# Should be different: tar_workspace() runs
# tar_seed_set(tar_meta(y, seed)$seed)
tail(.Random.seed)
})
}
```
