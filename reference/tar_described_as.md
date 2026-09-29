# Select targets using their descriptions.

Select a subset of targets in the `_targets.R` file based on their
custom descriptions.

## Usage

``` r
tar_described_as(
  described_as = NULL,
  tidyselect = TRUE,
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function),
  envir = parent.frame(),
  script = targets::tar_config_get("script")
)
```

## Arguments

- described_as:

  A `tidyselect` expression to select targets based on their
  descriptions. For example,
  `described_as = starts_with("survival model")` matches all targets in
  the pipeline whose `description` arguments of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  start with the text string `"survival model"`.

- tidyselect:

  If `TRUE`, return a call to
  [`tidyselect::all_of()`](https://tidyselect.r-lib.org/reference/all_of.html)
  identifying the selected targets, which can then be supplied to any
  `tidyselect`-compatible
  names`argument of downstream functions like [tar_make()] and [tar_manifest()]. If`FALSE\`,
  return a simple character vector of target names.

- callr_function:

  A function from `callr` to start a fresh clean R process to do the
  work. Set to `NULL` to run in the current session instead of an
  external process (but restart your R session just before you do in
  order to clear debris out of the global environment). `callr_function`
  needs to be `NULL` for interactive debugging, e.g.
  `tar_option_set(debug = "your_target")`. However, `callr_function`
  should not be `NULL` for serious reproducible work.

- callr_arguments:

  A list of arguments to `callr_function`.

- envir:

  An environment, where to run the target R script (default:
  `_targets.R`) if `callr_function` is `NULL`. Ignored if
  `callr_function` is anything other than `NULL`. `callr_function`
  should only be `NULL` for debugging and testing purposes, not for
  serious runs of a pipeline, etc.

  The `envir` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and related functions always overrides the current value of
  `tar_option_get("envir")` in the current R session just before running
  the target script file, so whenever you need to set an alternative
  `envir`, you should always set it with
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  from within the target script file. In other words, if you call
  `tar_option_set(envir = envir1)` in an interactive session and then
  `tar_make(envir = envir2, callr_function = NULL)`, then `envir2` will
  be used.

- script:

  Character of length 1, path to the target script file. Defaults to
  `tar_config_get("script")`, which in turn defaults to `_targets.R`.
  When you set this argument, the value of `tar_config_get("script")` is
  temporarily changed for the current function call. See
  [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md),
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about the target script file and how to set it
  persistently for a project.

## Value

If `tidyselect` is `TRUE`, then `tar_described_as()` returns a call to
[`tidyselect::all_of()`](https://tidyselect.r-lib.org/reference/all_of.html)
which can be supplied to the `names` argument of functions like
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
and
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
This allows functions like
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
and
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
to focus on only the targets with the matching descriptions. If
`tidyselect` is `FALSE`, then `tar_described_as()` returns a simple
character vector of the names of all the targets in the pipeline with
matching descriptions.

## Details

Targets with empty descriptions are ignored.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
[`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md),
[`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md),
[`tar_definition()`](https://docs.ropensci.org/targets/reference/tar_definition.md),
[`tar_envir()`](https://docs.ropensci.org/targets/reference/tar_envir.md),
[`tar_format_get()`](https://docs.ropensci.org/targets/reference/tar_format_get.md),
[`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.md),
[`tar_name()`](https://docs.ropensci.org/targets/reference/tar_name.md),
[`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md),
[`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md),
[`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md),
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md),
[`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(b2, TRUE, description = "blue two"),
    tar_target(b3, TRUE, description = "blue three"),
    tar_target(g2, TRUE, description = "green two"),
    tar_target(g3, TRUE, description = "green three"),
    tar_target(g4, TRUE, description = "green three")
  )
}, ask = FALSE)
tar_described_as(starts_with("green"), tidyselect = FALSE)
tar_make(names = tar_described_as(starts_with("green")))
tar_progress() # Only `g2`, `g3`, and `g4` ran.
})
}
```
