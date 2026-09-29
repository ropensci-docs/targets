# Check existence in local CAS.

For internal use only.

## Usage

``` r
tar_cas_e(cas, key)
```

## Arguments

- cas:

  File path to the CAS repository. `NULL` to default to
  `file.path(tar_config_get("store"), "cas")` (usually
  `"_targets/cas/"`).

- key:

  Key of the object in the CAS system.

## Value

Character vector of keys (metadata hashes) found in the CAS system.

## Details

The short function name helps reduce the size of the
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
format string and save space in the metadata.
