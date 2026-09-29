# Get main process ID.

Get the process ID (PID) of the most recent main R process to
orchestrate the targets of the current project.

## Usage

``` r
tar_pid(store = targets::tar_config_get("store"))
```

## Arguments

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

Integer with the process ID (PID) of the most recent main R process to
orchestrate the targets of the current project.

## Details

The main process is the R process invoked by
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
or similar. If `callr_function` is not `NULL`, this is an external
process, and the `pid` in the return value will not agree with
[`Sys.getpid()`](https://rdrr.io/r/base/Sys.getpid.html) in your current
interactive session. The process may or may not be alive. You may want
to check it with `ps::ps_is_running(ps::ps_handle(targets::tar_pid()))`
before running another call to
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
for the same project.

## See also

Other data:
[`tar_crew()`](https://docs.ropensci.org/targets/reference/tar_crew.md),
[`tar_process()`](https://docs.ropensci.org/targets/reference/tar_process.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  list(
    tar_target(x, seq_len(2)),
    tar_target(y, 2 * x, pattern = map(x))
  )
}, ask = FALSE)
tar_make()
Sys.getpid()
tar_pid() # Different from the current PID.
})
}
```
