# Check which targets are outdated.

Checks for outdated targets in the pipeline, targets that will be rerun
automatically if you call
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
or similar. See
[`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
for the rules that decide whether a target needs to rerun.

## Usage

``` r
tar_outdated(
  names = NULL,
  shortcut = targets::tar_config_get("shortcut"),
  branches = FALSE,
  targets_only = TRUE,
  reporter = targets::tar_config_get("reporter_outdated"),
  seconds_reporter = targets::tar_config_get("seconds_reporter_outdated"),
  seconds_interval = targets::tar_config_get("seconds_interval"),
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function, reporter),
  envir = parent.frame(),
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Names of the targets. `tar_outdated()` will check these targets and
  all upstream ancestors in the dependency graph. Set `names` to `NULL`
  to check/build all the targets (default). The object supplied to
  `names` should be `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- shortcut:

  Logical of length 1, how to interpret the `names` argument. If
  `shortcut` is `FALSE` (default) then the function checks all targets
  upstream of `names` as far back as the dependency graph goes. If
  `TRUE`, then the function only checks the targets in `names` and uses
  stored metadata for information about upstream dependencies as needed.
  `shortcut = TRUE` increases speed if there are a lot of up-to-date
  targets, but it assumes all the dependencies are up to date, so please
  use with caution. Also, `shortcut = TRUE` only works if you set
  `names`.

- branches:

  Logical of length 1, whether to include branch names. Including
  branches could get cumbersome for large pipelines. Individual branch
  names are still omitted when branch-specific information is not
  reliable: for example, when a pattern branches over an outdated
  target.

- targets_only:

  Logical of length 1, whether to just restrict to targets or to include
  functions and other global objects from the environment created by
  running the target script file (default: `_targets.R`).

- reporter:

  Character of length 1, name of the reporter to user. Controls how
  messages are printed as targets are checked.

  The default value of `reporter` is the value returned by
  `tar_config_get("reporter_outdated")`. The default of
  `tar_config_get("reporter_outdated")` is `"terse"` if the calling R
  session is either:

        1. Non-interactive (`interactive()` returns `FALSE`), or
        2. Inside a literate programming document
          (the `knitr.in.progress` global option is `TRUE`).

  Otherwise, the default is `"balanced"`. You can always set the
  reporter manually. Choices:

      * `"balanced"`: a reporter that balances efficiency
        with informative detail.
        Uses a `cli` progress bar instead of printing messages
        for individual dynamic branches.
        To the right of the progress bar is a text string like
        "22.6s, 4510+, 124-" (22.6 seconds elapsed, 4510 targets
        detected as outdated so far,
        124 targets detected as up to date so far).

        For best results with the balanced reporter, you may need to
        adjust your `cli` settings. See global options `cli.num_colors`
        and `cli.dynamic` at
        <https://cli.r-lib.org/reference/cli-config.html>.
        On that page is also the `CLI_TICK_TIME` environment variable
        which controls the time delay between progress bar updates.
        If the delay is too low, then overhead from printing to the console
        may slow down the pipeline.
      * `"terse"`: like `"balanced"`, except without a progress bar.
      * `"silent"`: print nothing.

- seconds_reporter:

  Deprecated on 2025-03-31 (`targets` version 1.10.1.9010).

- seconds_interval:

  Deprecated on 2023-08-24 (targets version 1.2.2.9001). Use
  `seconds_meta_append` and `seconds_meta_upload` instead.

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

Names of the outdated targets.

## Details

Requires that you define a pipeline with a target script file (default:
`_targets.R`). (See
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)
for details.)

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

Other inspect:
[`tar_deps()`](https://docs.ropensci.org/targets/reference/tar_deps.md),
[`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md),
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md),
[`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md),
[`tar_sitrep()`](https://docs.ropensci.org/targets/reference/tar_sitrep.md),
[`tar_validate()`](https://docs.ropensci.org/targets/reference/tar_validate.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(list(tar_target(x, 1 + 1)))
tar_outdated()
tar_script({
  library(targets)
  library(tarchetypes)
  list(
    tar_target(y1, 1 + 1),
    tar_target(y2, 1 + 1),
    tar_target(z, y1 + y2)
  )
}, ask = FALSE)
tar_outdated()
})
}
```
