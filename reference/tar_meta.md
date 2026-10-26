# Read a project's metadata.

Read the metadata of all recorded targets and global objects.

## Usage

``` r
tar_meta(
  names = NULL,
  fields = NULL,
  targets_only = FALSE,
  complete_only = FALSE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Optional, names of the targets. If supplied, `tar_meta()` only returns
  metadata on these targets. You can supply symbols or `tidyselect`
  helpers like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) and
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).
  If `NULL`, all names are selected.

- fields:

  Optional, names of columns/fields to select. If supplied, `tar_meta()`
  only returns the selected metadata columns. If `NULL`, all fields are
  selected. You can supply symbols or `tidyselect` helpers like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) and
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).
  The `name` column is always included first no matter what you select.
  Choices:

  - `name`: name of the target or global object.

  - `type`: type of the object: either `"function"` or `"object"` for
    global objects, and `"stem"`, `"branch"`, `"map"`, or `"cross"` for
    targets.

  - `data`: hash of the output data.

  - `command`: hash of the target's deparsed command.

  - `depend`: hash of the immediate upstream dependencies of the target.

  - `seed`: random number generator seed with which the target ran. A
    target's random number generator seed is a deterministic function of
    its name. In this way, each target runs with a reproducible seed so
    someone else running the same pipeline should get the same results,
    and no two targets in the same pipeline share the same seed. (Even
    dynamic branches have different names and thus different seeds.) You
    can recover the seed of a completed target with
    `tar_meta(your_target, seed)` and run
    [`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md)
    on the result to locally recreate the target's initial RNG state.

  - `path`: A list column of paths to target data. Usually, each element
    is a single path, but there could be multiple paths per target for
    file targets (i.e. `tar_target(format = "file")`).

  - `time`: `POSIXct` object with the time the target's data in storage
    was last modified. If the target stores no local file, then the time
    stamp corresponds to the time the target last ran successfully. Only
    targets that run commands have time stamps: just non-branching
    targets and individual dynamic branches. Displayed in the current
    time zone of the system. If there are multiple outputs for that
    target, as with file targets, then the maximum time is shown.

  - `size`: hash of the sum of all the bytes of the files at `path`.

  - `bytes`: total file size in bytes of all files in `path`.

  - `format`: character, one of the admissible data storage formats. See
    the `format` argument in the
    [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
    help file for details.

  - `iteration`: character, either `"list"` or `"vector"` to describe
    the iteration and aggregation mode of the target. See the
    `iteration` argument in the
    [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
    help file for details.

  - `parent`: for branches, name of the parent pattern.

  - `children`: list column, names of the children of targets that have
    them. These include buds of stems and branches of patterns.

  - `seconds`: number of seconds it took to run the target.

  - `warnings`: character string of warning messages from the last run
    of the target. Only the first 50 warnings are available, and only
    the first 2048 characters of the concatenated warning messages.

  - `error`: character string of the error message if the target
    errored.

- targets_only:

  Logical, whether to just show information about targets or also return
  metadata on functions and other global objects.

- complete_only:

  Logical, whether to return only complete rows (no `NA` values).

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

A data frame with one row per target/object and the selected fields.

## Details

A metadata row only updates when the target completes.
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
shows information on targets that are running. That is why the number of
branches may disagree between `tar_meta()` and
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
for actively running pipelines.

## Storage access

Several functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md),
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
`tar_meta()`, and
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

## Cloud metadata

Metadata files help `targets` read data objects and decide if the
pipeline is up to date. Usually, these metadata files live in files in
the local `_targets/meta/` folder in your project, e.g.
`_targets/meta/meta`. But in addition, if you set `repository` to
anything other than `"local"` in
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
in `_targets.R`, then
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
continuously uploads the metadata files to the bucket you specify in
`resources`.
[`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md)
will delete those files from the cloud, and so will
[`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
if `destroy` is set to either `"all"` or `"cloud"`.

Other functions in `targets`, such as `tar_meta()`,
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md),
and
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md),
use the local metadata only and ignore the copies on the cloud. So if
you are working on a different computer than the one running the
pipeline, you will need to download the cloud metadata to your current
machine using
[`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md).
Other functions
[`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md),
[`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md),
and
[`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md)
also manage metadata across the cloud and the local file system.

Remarks:

- The `repository_meta` option in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  is actually what controls where the metadata lives in the cloud, but
  it defaults to `repository`.

- Like
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  and
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  also continuously upload metadata files to the cloud bucket specified
  in `resources`.

- [`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md)
  and related functions need to run `_targets.R` to detect
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  options `repository_meta` and `resources`, so please be aware of side
  effects that may happen running your custom `_targets.R` file.

## See also

Other metadata:
[`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md),
[`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md),
[`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md),
[`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(x, seq_len(2)),
    tar_target(y, 2 * x, pattern = map(x))
  )
}, ask = FALSE)
tar_make()
tar_meta()
tar_meta(starts_with("y_")) # see also any_of()
})
}
```
