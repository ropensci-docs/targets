# Delete target output values.

Delete the output values of targets in `_targets/objects/` (or the cloud
if applicable) but keep the records in the metadata.

## Usage

``` r
tar_delete(
  names,
  cloud = TRUE,
  batch_size = 1000L,
  verbose = TRUE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Optional, names of the targets to delete. If supplied, the `names`
  argument restricts the targets which are deleted. The value is a
  `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

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

If you have a small number of data-heavy targets you need to discard to
conserve storage, this function can help. Local external files files
(i.e. `format = "file"` and `repository = "local"`) are not deleted. For
targets with `repository` not equal `"local"`, `tar_delete()` attempts
to delete the file and errors out if the deletion is unsuccessful. If
deletion fails, either log into the cloud platform and manually delete
the file (e.g. the AWS web console in the case of `repository = "aws"`)
or call
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md)
on that target so that `targets` does not try to delete the object. For
patterns recorded in the metadata, all the branches will be deleted. For
patterns no longer in the metadata, branches are left alone.

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
functions `tar_delete()` and
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

Other clean:
[`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md),
[`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md),
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
tar_delete(starts_with("y")) # Only deletes y1 and y2.
tar_make() # y1 and y2 rerun but return the same values, so z is up to date.
})
}
```
