# Deduplicate meta and progress databases (deprecated).

Deprecated in `targets` version 0.3.0 (2020-03-06). Deduplication
happens automatically before and after the pipeline runs.

## Usage

``` r
tar_deduplicate(
  meta = TRUE,
  progress = TRUE,
  store = targets::tar_config_get("store")
)
```

## Arguments

- meta:

  Logical, whether to deduplicate the meta database file at
  `_targets/meta/meta`.

- progress:

  Logical, whether to deduplicate the progress database file at
  `_targets/meta/progress`.

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

Nothing.

## Details

Removes duplicated entries in the meta and progress databases in order
to lighten storage. These databases are located in the
`_targets/meta/meta` and `_targets/meta/progress` files, where
`_targets` is the a folder at the project root. No essential data is
removed, so this is simply a form of garbage collection.
