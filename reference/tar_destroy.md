# Destroy the data store.

Destroy the data store written by the pipeline.

## Usage

``` r
tar_destroy(
  destroy = c("all", "cloud", "local", "meta", "process", "progress", "objects",
    "scratch", "workspaces", "user"),
  batch_size = 1000L,
  verbose = TRUE,
  ask = NULL,
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- destroy:

  Character of length 1, what to destroy. Choices:

  - `"all"`: entire data store (default: `_targets/`) including cloud
    data, as well as download/upload scratch files.

  - `"cloud"`: cloud data, including metadata, target definition object
    data from targets with `tar_target(..., repository = "aws")`, and
    workspace files saved on the cloud. Also deletes temporary staging
    files in `file.path(tempdir(), "targets")` that may have been
    accidentally left over from incomplete uploads or downloads.

  - `"local"`: all the local files in the data store but nothing on the
    cloud.

  - `"meta"`: metadata file at `meta/meta` in the data store, which
    invalidates all the targets but keeps the data.

  - `"process"`: progress data file at `meta/process` in the data store,
    which resets the metadata of the main process.

  - `"progress"`: progress data file at `meta/progress` in the data
    store, which resets the progress tracking info.

  - `"objects"`: all the target return values in `objects/` in the data
    store but keep progress and metadata. Dynamic files are not deleted
    this way.

  - `"scratch"`: temporary files in saved during
    [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
    that should automatically get deleted except if R crashed.

  - `"workspaces"`: compressed lightweight files locally saved to the
    `workspaces/` folder in the data store with the saved workspaces of
    targets. Does not delete workspace files on the cloud. For that,
    consider `destroy = "all"` or `destroy = "cloud"`. See
    [`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
    for details.

  - `"user"`: custom user-supplied files in the `user/` folder in the
    data store.

- batch_size:

  Positive integer between 1 and 1000, number of target definition
  objects to delete from the cloud with each HTTP API request. Currently
  only supported for AWS. Cannot be more than 1000.

- verbose:

  Logical of length 1, whether to print console messages to show
  progress when deleting each batch of targets from each cloud bucket.
  Batched deletion with verbosity is currently only supported for AWS.

- ask:

  Logical of length 1, whether to pause with a menu prompt before
  deleting files. To disable this menu, set the `TAR_ASK` environment
  variable to `"false"`.
  [`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html)
  can help set environment variables.

- script:

  Character of length 1, path to the target script file. Defaults to
  `tar_config_get("script")`, which in turn defaults to `_targets.R`. If
  the script does not exist, then cloud metadata will not be deleted.

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

`NULL` (invisibly).

## Details

The data store is a folder created by
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
(or
[`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
or
[`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)).
The details of the data store are explained at
<https://books.ropensci.org/targets/data.html#local-data-store>. The
data store folder contains the output data and metadata of the targets
in the pipeline. Usually, the data store is a folder called `_targets/`
(see
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
to customize), and it may link to data on the cloud if you used AWS or
GCP buckets. By default, `tar_destroy()` deletes the entire `_targets/`
folder (or wherever the data store is located), including custom
user-supplied files in `_targets/user/`, as well as any cloud data that
the pipeline uploaded. See the `destroy` argument to customize this
behavior and only delete part of the data store, and see functions like
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md),
[`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
and
[`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
to remove information pertaining to some but not all targets in the
pipeline. After calling `tar_destroy()` with default arguments, the
entire data store is gone, which means all the output data from previous
runs of the pipeline is gone (except for input/output files tracked with
`tar_target(..., format = "file")`). The next run of the pipeline will
start from scratch, and it will not skip any targets.

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
and `tar_destroy()` only remove the version ID of each target as
recorded in the local metadata.

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
    and `tar_destroy()` will use the *latest* version of each target
    data object.

4.  Optional: to back up the local metadata file with the version IDs
    deleted, use
    [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md).

## See also

Other clean:
[`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
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
  list(tar_target(x, 1 + 1))
})
tar_make() # Creates the _targets/ data store.
tar_destroy()
print(file.exists("_targets")) # Should be FALSE.
})
}
```
