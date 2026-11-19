# Load the values of all available targets.

Shorthand for `tar_load(everything())` to load all targets with entries
in the metadata.

## Usage

``` r
tar_load_everything(
  branches = NULL,
  meta = targets::tar_meta(store = store, fields = -tidyselect::any_of("time")),
  strict = TRUE,
  silent = FALSE,
  envir = parent.frame(),
  store = targets::tar_config_get("store")
)
```

## Arguments

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

## See also

Other storage:
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md),
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
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
tar_load_everything()
ls() # Has "y1", "y2", and "z".
})
}
```
