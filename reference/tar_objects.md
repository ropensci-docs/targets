# List saved targets

List targets currently saved to `_targets/objects/` or the cloud. Does
not include local files with
`tar_target(..., format = "file", repository = "local")`.

## Usage

``` r
tar_objects(
  names = NULL,
  cloud = TRUE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Names of targets to select. The object supplied to `names` should be
  `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- cloud:

  Logical of length 1, whether to include cloud targets in the output
  (e.g. `tar_target(..., repository = "aws")`).

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

Character vector of targets saved to `_targets/objects/`.

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

Other storage:
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md),
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
[`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(tar_target(x, "value"))
}, ask = FALSE)
tar_make()
tar_objects()
tar_objects(starts_with("x")) # see also any_of()
})
}
```
