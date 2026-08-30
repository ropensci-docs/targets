# Delete one or more metadata records (e.g. to rerun a target).

Delete the metadata of records in `_targets/meta/meta` but keep the
return values of targets in `_targets/objects/`.

## Usage

``` r
tar_invalidate(names, store = targets::tar_config_get("store"))
```

## Arguments

- names:

  Names of the targets to remove from the metadata list. The object
  supplied to `names` should be a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

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

This function forces one or more targets to rerun on the next
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
regardless of the cues and regardless of how those targets are stored.
After `tar_invalidate()`, you will still be able to locate the data
files with
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md)
and manually salvage them in an emergency. However,
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
and
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
will not be able to read the data into R, and subsequent calls to
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
will attempt to rerun those targets. For patterns recorded in the
metadata, all the branches will be invalidated. For patterns no longer
in the metadata, branches are left alone.

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

## See also

Other clean:
[`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
[`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
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
tar_invalidate(starts_with("y")) # Only invalidates y1 and y2.
tar_make() # y1 and y2 rerun but return same values, so z is up to date.
})
}
```
