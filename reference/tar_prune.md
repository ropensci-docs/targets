# Remove targets that are no longer part of the pipeline.

Remove target values from `_targets/objects/` and the cloud and remove
target metadata from `_targets/meta/meta` for targets that are no longer
part of the pipeline.

## Usage

``` r
tar_prune(
  cloud = TRUE,
  batch_size = 1000L,
  verbose = TRUE,
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function),
  envir = parent.frame(),
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- cloud:

  Logical of length 1, whether to delete objects from the cloud if
  applicable (e.g. AWS, GCP). If `FALSE`, files are not deleted from the
  cloud.

- batch_size:

  Positive integer between 1 and 1000, number of target definition
  objects to delete from the cloud with each HTTP API request. Currently
  only supported for AWS. Cannot be more than 1000.

- verbose:

  Logical of length 1, whether to print console messages to show
  progress when deleting each batch of targets from each cloud bucket.
  Batched deletion with verbosity is currently only supported for AWS.

- callr_function:

  A function from `callr` to start a fresh clean R process to do the
  work. Set to `NULL` to run in the current session instead of an
  external process (but restart your R session just before you do in
  order to clear debris out of the global environment). `callr_function`
  needs to be `NULL` for interactive debugging, e.g.
  `tar_option_set(debug = "your_target")`. However, `callr_function`
  should not be `NULL` for serious reproducible work.

- callr_arguments:

  A list of arguments to `callr_function`.

- envir:

  An environment, where to run the target R script (default:
  `_targets.R`) if `callr_function` is `NULL`. Ignored if
  `callr_function` is anything other than `NULL`. `callr_function`
  should only be `NULL` for debugging and testing purposes, not for
  serious runs of a pipeline, etc.

  The `envir` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and related functions always overrides the current value of
  `tar_option_get("envir")` in the current R session just before running
  the target script file, so whenever you need to set an alternative
  `envir`, you should always set it with
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  from within the target script file. In other words, if you call
  `tar_option_set(envir = envir1)` in an interactive session and then
  `tar_make(envir = envir2, callr_function = NULL)`, then `envir2` will
  be used.

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

`NULL` except if `callr_function` is
[`callr::r_bg`](https://callr.r-lib.org/reference/r_bg.html), in which
case a handle to the `callr` background process is returned. Either way,
the value is invisibly returned.

## Details

`tar_prune()` is useful if you recently worked through multiple changes
to your project and are now trying to discard irrelevant data while
keeping the results that still matter. Global objects and local files
with `format = "file"` outside the data store are unaffected. Also
removes `_targets/scratch/`, which is only needed while
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
[`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
or
[`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
is running. To list the targets that will be pruned without actually
removing anything, use
[`tar_prune_list()`](https://docs.ropensci.org/targets/reference/tar_prune_list.md).

## Storage access

Several functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md),
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
[`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md),
and
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
read or modify the local data store of the pipeline. The local data
store is in flux while a pipeline is running, and depending on how
distributed computing or cloud computing is set up, not all targets can
even reach it. So please do not call these functions from inside a
target as part of a running pipeline. The only exception is literate
programming target factories in the `tarchetypes` package such as
[`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.html)
and
[`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.html).

## Cloud target data versioning

Some buckets in Amazon S3 or Google Cloud Storage are "versioned", which
means they track historical versions of each data object. If you use
`targets` with cloud storage
(<https://books.ropensci.org/targets/cloud-storage.html>) and versioning
is turned on, then `targets` will record each version of each target in
its metadata.

Functions like
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
and
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
load the version recorded in the local metadata, which may not be the
same as the "current" version of the object in the bucket. Likewise,
functions
[`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md)
and
[`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
only remove the version ID of each target as recorded in the local
metadata.

If you want to interact with the *latest* version of an object instead
of the version ID recorded in the local metadata, then you will need to
delete the object from the metadata.

1.  Make sure your local copy of the metadata is current and up to date.
    You may need to run
    [`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md)
    or
    [`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md)
    first.

2.  Run
    [`tar_unversion()`](https://docs.ropensci.org/targets/reference/tar_unversion.md)
    to remove the recorded version IDs of your targets in the local
    metadata.

3.  With the version IDs gone from the local metadata, functions like
    [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
    and
    [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
    will use the *latest* version of each target data object.

4.  Optional: to back up the local metadata file with the version IDs
    deleted, use
    [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md).

## See also

tar_prune_inspect

Other clean:
[`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
[`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md),
[`tar_prune_list()`](https://docs.ropensci.org/targets/reference/tar_prune_list.md),
[`tar_unversion()`](https://docs.ropensci.org/targets/reference/tar_unversion.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(y1, 1 + 1),
    tar_target(y2, 1 + 1),
    tar_target(z, y1 + y2)
  )
}, ask = FALSE)
tar_make()
# Remove some targets from the pipeline.
tar_script(list(tar_target(y1, 1 + 1)), ask = FALSE)
# Keep only the remaining targets in the data store.
tar_prune()
})
}
```
