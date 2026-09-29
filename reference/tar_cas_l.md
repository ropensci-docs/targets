# List keys in local CAS.

For internal use only.

## Usage

``` r
tar_cas_l(cas, keys)
```

## Arguments

- cas:

  File path to the CAS repository. `NULL` to default to
  `file.path(tar_config_get("store"), "cas")` (usually
  `"_targets/cas/"`).

- keys:

  Character vector of keys in the metadata hashes (`tar_meta()$data`).
  Used to restrict the output of the return value to avoid listing all
  the potentially millions of files in the CAS system.

## Value

Character vector of keys (metadata hashes) found in the CAS system.

## Details

The short function name helps reduce the size of the
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
format string and save space in the metadata.
