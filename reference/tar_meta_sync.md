# Synchronize cloud metadata.

Synchronize metadata in a cloud bucket with metadata in the local data
store.

## Usage

``` r
tar_meta_sync(
  meta = TRUE,
  progress = TRUE,
  process = TRUE,
  crew = TRUE,
  verbose = TRUE,
  prefer_local = TRUE,
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

- prefer_local:

  Logical of length 1 to control which copy of each metadata file takes
  precedence if the local hash and cloud hash are different but the time
  stamps are the same. Set to `TRUE` to upload the local data file in
  that scenario, `FALSE` to download the cloud file.

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

## Details

`tar_meta_sync()` synchronizes the local and cloud copies of all the
metadata files of the pipeline so that both have the most recent copy.
For each metadata file, if the local file does not exist or is older
than the cloud file, then the cloud file is downloaded to the local file
path. Conversely, if the cloud file is older or does not exist, then the
local file is uploaded to the cloud. If the time stamps of these files
are equal, use the `prefer_local` argument to determine which copy takes
precedence.

## See also

Other metadata:
[`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md),
[`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md),
[`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md),
[`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md)

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
tar_meta_sync()
})
}
```
