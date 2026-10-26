# Delete cloud object version IDs from local metadata.

Delete version IDs from local metadata.

## Usage

``` r
tar_unversion(
  names = tidyselect::everything(),
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Tidyselect expression to identify the targets to drop version IDs. The
  object supplied to `names` should be `NULL` or a `tidyselect`
  expression like
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

2.  Run `tar_unversion()` to remove the recorded version IDs of your
    targets in the local metadata.

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
[`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
[`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
[`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md),
[`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md),
[`tar_prune_list()`](https://docs.ropensci.org/targets/reference/tar_prune_list.md)
