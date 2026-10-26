# Load the values of targets.

Load the return values of targets into the current environment (or the
environment of your choosing). For a typical target, the return value
lives in a file in `_targets/objects/`. For file targets (i.e.
`format = "file"`) the paths loaded in place of the values.
[`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md)
is shorthand for `tar_load(everything())` to load all targets.

`tar_load()` uses non-standard evaluation in the `names` argument
(example: `tar_load(names = everything())`), whereas `tar_load_raw()`
uses standard evaluation for `names` (example:
`tar_load_raw(names = quote(everything()))`).

## Usage

``` r
tar_load(
  names,
  branches = NULL,
  meta = targets::tar_meta(store = store, fields = -tidyselect::any_of("time")),
  strict = TRUE,
  silent = FALSE,
  envir = parent.frame(),
  store = targets::tar_config_get("store")
)

tar_load_raw(
  names,
  branches = NULL,
  meta = targets::tar_meta(store = store, fields = -tidyselect::any_of("time")),
  strict = TRUE,
  silent = FALSE,
  envir = parent.frame(),
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Names of the targets to load. `tar_load()` uses non-standard
  evaluation in the `names` argument (example:
  `tar_load(names = everything())`), whereas `tar_load_raw()` uses
  standard evaluation for `names` (example:
  `tar_load_raw(names = quote(everything()))`).

  The object supplied to `names` should be a `tidyselect` expression
  like [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html)
  or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- branches:

  Integer of indices of the branches to load for any targets that are
  patterns.

- meta:

  Data frame of target metadata from
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md).

- strict:

  Logical of length 1, whether to error out if one of the selected
  targets is in the metadata but cannot be loaded. Set to `FALSE` to
  just load the targets in the metadata that can be loaded and skip the
  others.

- silent:

  Logical of length 1. Only relevant when `strict` is `FALSE`. If
  `silent` is `FALSE` and `strict` is `FALSE`, then a message will be
  printed if a target is in the metadata but cannot be loaded. However,
  load failures will not stop other targets from being loaded.

- envir:

  R environment in which to load target return values.

- store:

  Character of length 1, directory path to the data store of the
  pipeline.

## Value

Nothing.

## Storage access

Several functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md),
`tar_load()`,
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
and `tar_load()` load the version recorded in the local metadata, which
may not be the same as the "current" version of the object in the
bucket. Likewise, functions
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

Other storage:
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md),
[`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md),
[`tar_objects()`](https://docs.ropensci.org/targets/reference/tar_objects.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)

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
ls() # Does not have "y1", "y2", or "z".
tar_load(starts_with("y"))
ls() # Has "y1" and "y2" but not "z".
tar_load_raw(quote(any_of("z")))
ls() # Has "y1", "y2", and "z".
})
}
```
