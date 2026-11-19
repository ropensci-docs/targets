# Upload local metadata to the cloud.

Upload local metadata files to the cloud location (repository, bucket,
and prefix) you set in
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
in `_targets.R`.

## Usage

``` r
tar_meta_upload(
  meta = TRUE,
  progress = TRUE,
  process = TRUE,
  crew = TRUE,
  verbose = TRUE,
  strict = FALSE,
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- meta:

  Logical of length 1, whether to process the main metadata file at
  `_targets/meta/meta`.

- progress:

  Logical of length 1, whether to process the progress file at
  `_targets/meta/progress`.

- process:

  Logical of length 1, whether to process the process file at
  `_targets/meta/process`.

- crew:

  Logical of length 1, whether to process the `crew` file at
  `_targets/meta/crew`. Only exists if running `targets` with `crew`.

- verbose:

  Logical of length 1, whether to print informative console messages.

- strict:

  Logical of length 1. `TRUE` to error out if the file does not exist
  locally, `FALSE` to proceed without an error or warning. If `strict`
  is `FALSE` and `verbose` is `TRUE`, then an informative message will
  print to the R console.

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

## See also

Other metadata:
[`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md),
[`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md),
[`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md),
[`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(
    resources = tar_resources(
      aws = tar_resources_aws(
        bucket = "YOUR_BUCKET_NAME",
        prefix = "YOUR_PROJECT_NAME"
      )
    ),
    repository = "aws"
  )
  list(
    tar_target(x, data.frame(x = seq_len(2), y = seq_len(2)))
  )
}, ask = FALSE)
tar_make()
tar_meta_upload()
})
}
```
