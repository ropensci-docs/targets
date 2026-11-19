# Download a workspace from the cloud.

Download a workspace file from the cloud so it can be loaded with
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md).

## Usage

``` r
tar_workspace_download(
  name,
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store"),
  verbose = TRUE
)
```

## Arguments

- name:

  Symbol, name of the target whose workspace to download.

- script:

  Character string, file path to the `_targets.R` file defining the
  pipeline. Must be configured with the right `aws` and
  `repository_meta` options (in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md))
  to support downloading workspaces from the cloud.

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

- verbose:

  `TRUE` to print an informative message describing the download,
  `FALSE` to stay silent.

## Value

`NULL` (invisibly). Returns an error if the workspace cannot be
downloaded.

## Details

If `tar_option_get("repository_meta")` is `"aws"` or `"gcp"`, then
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
uploads workspaces to the bucket and prefix provided. Download one of
these workspaces with `tar_workspace_download()`. Downloaded workspaces
can be loaded the usual way with
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md),
and you should see them in character vector returned by
[`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md).

## See also

Other debug:
[`tar_load_globals()`](https://docs.ropensci.org/targets/reference/tar_load_globals.md),
[`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md),
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md),
[`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tmp <- sample(1)
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(
    resources = tar_resources(
      tar_resources_aws(
        bucket = "YOUR_AWS_BUCKET",
        prefix = "_targets"
      )
    ),
    repository = "aws",
    repository_meta = "aws"
  )
  list(
    tar_target(x, stop("this is an error and thus triggers a workspace"))
  )
}, ask = FALSE)
# The following code throws an error for demonstration purposes.
try(tar_make())
# Say the workspace file for target x does not exist.
unlink("_targets/workspaces/x")
file.exists("_targets/workspaces/x")
# We can download it with tar_workspace_download()
tar_workspace_download(x)
file.exists("_targets/workspaces/x")
tar_workspace(x)
})
}
```
