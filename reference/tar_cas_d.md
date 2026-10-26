# Local CAS download.

For internal use only.

## Usage

``` r
tar_cas_d(cas, key, path)
```

## Arguments

- cas:

  File path to the CAS repository. `NULL` to default to
  `file.path(tar_config_get("store"), "cas")` (usually
  `"_targets/cas/"`).

- key:

  Key of the object in the CAS system.

- path:

  Staging path of the file.

## Value

Called for its side effects.
