# Read a target's value from storage.

Read a target's return value from its file in `_targets/objects/`. For
file targets (i.e. `format = "file"`) the paths are returned.

`tar_read()` expects an unevaluated symbol for the `name` argument,
whereas `tar_read_raw()` expects a character string.

## Usage

``` r
tar_read(
  name,
  branches = NULL,
  meta = targets::tar_meta(store = store, fields = -tidyselect::any_of("time")),
  store = targets::tar_config_get("store")
)

tar_read_raw(
  name,
  branches = NULL,
  meta = targets::tar_meta(store = store, fields = -tidyselect::any_of("time")),
  store = targets::tar_config_get("store")
)
```

## Arguments

- name:

  Name of the target to read. `tar_read()` expects an unevaluated symbol
  for the `name` argument, whereas `tar_read_raw()` expects a character
  string.

- branches:

  Integer of indices of the branches to load if the target is a pattern.

- meta:

  Data frame of metadata from
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md).
  `tar_read()` with the default arguments can be inefficient for large
  pipelines because all the metadata is stored in a single file.
  However, if you call
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md)
  beforehand and supply it to the `meta` argument, then successive calls
  to `tar_read()` may run much faster.

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

The target's return value from its file in `_targets/objects/`, or the
paths to the custom files and directories if `format = "file"` was set.

## Cloud target data versioning

Some buckets in Amazon S3 or Google Cloud Storage are "versioned", which
means they track historical versions of each data object. If you use
`targets` with cloud storage
(<https://books.ropensci.org/targets/cloud-storage.html>) and versioning
is turned on, then `targets` will record each version of each target in
its metadata.

Functions like `tar_read()` and
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
    `tar_read()` and
    [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
    will use the *latest* version of each target data object.

4.  Optional: to back up the local metadata file with the version IDs
    deleted, use
    [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md).

## Storage access

Several functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
`tar_read()`,
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

## See also

Other storage:
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md),
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
[`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md),
[`tar_objects()`](https://docs.ropensci.org/targets/reference/tar_objects.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(tar_target(x, 1 + 1))
})
tar_make()
tar_read(x)
tar_read_raw("x")
})
}
```
