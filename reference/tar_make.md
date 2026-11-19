# Run a pipeline of targets.

Run the pipeline you defined in the targets script file (default:
`_targets.R`). `tar_make()` runs the correct targets in the correct
order and stores the return values in `_targets/objects/`. Use
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
to read a target back into R, and see
<https://docs.ropensci.org/targets/reference/index.html#clean> to manage
output files.

## Usage

``` r
tar_make(
  names = NULL,
  shortcut = targets::tar_config_get("shortcut"),
  reporter = targets::tar_config_get("reporter_make"),
  seconds_meta_append = targets::tar_config_get("seconds_meta_append"),
  seconds_meta_upload = targets::tar_config_get("seconds_meta_upload"),
  seconds_reporter = targets::tar_config_get("seconds_reporter"),
  seconds_interval = targets::tar_config_get("seconds_interval"),
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function, reporter),
  envir = parent.frame(),
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store"),
  garbage_collection = NULL,
  use_crew = targets::tar_config_get("use_crew"),
  terminate_controller = TRUE,
  as_job = targets::tar_config_get("as_job")
)
```

## Arguments

- names:

  Names of the targets to run or check. Set to `NULL` to check/run all
  the targets (default). The object supplied to `names` should be a
  `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- shortcut:

  Logical of length 1, how to interpret the `names` argument. If
  `shortcut` is `FALSE` (default) then the function checks all targets
  upstream of `names` as far back as the dependency graph goes.
  `shortcut = TRUE` increases speed if there are a lot of up-to-date
  targets, but it assumes all the dependencies are up to date, so please
  use with caution. It relies on stored metadata for information about
  upstream dependencies. `shortcut = TRUE` only works if you set
  `names`.

- reporter:

  Character of length 1, name of the reporter to user. Controls how
  messages are printed as targets run in the pipeline.

  The default value of `reporter` is the value returned by
  `tar_config_get("reporter_make")`. The default of
  `tar_config_get("reporter_make")` is `"terse"` if the calling R
  session is either:

        1. Non-interactive (`interactive()` returns `FALSE`), or
        2. Inside a literate programming document
          (the `knitr.in.progress` global option is `TRUE`).

  Otherwise, the default is `"balanced"`. You can always set the
  reporter manually. Choices:

  - `"balanced"`: a reporter that balances efficiency with informative
    detail. Uses a `cli` progress bar instead of printing messages for
    individual dynamic branches. To the right of the progress bar is a
    text string like "22.6s, 4510+, 124-" (22.6 seconds elapsed, 4510
    targets completed successfully so far, 124 targets skipped so far).

    For best results with the balanced reporter, you may need to adjust
    your `cli` settings. See global options `cli.num_colors` and
    `cli.dynamic` at <https://cli.r-lib.org/reference/cli-config.html>.
    On that page is also the `CLI_TICK_TIME` environment variable which
    controls the time delay between progress bar updates. If the delay
    is too low, then overhead from printing to the console may slow down
    the pipeline.

  - `"terse"`: like the `"balanced"` reporter, but without a progress
    bar.

  - `"silent"`: print nothing.

  - `"timestamp"`: same as the `"verbose"` reporter except that each
    message begins with a time stamp.

  - `"verbose"`: print messages for individual targets as they dispatch
    or complete. Each individual target-specific time (e.g. "3.487
    seconds") is strictly the elapsed runtime of the target and does not
    include steps like data retrieval and output storage.

- seconds_meta_append:

  Positive numeric of length 1 with the minimum number of seconds
  between saves to the local metadata and progress files in the data
  store. his is an aggressive optimization setting not recommended for
  most users: higher values generally make the pipeline run faster, but
  unsaved work (in the event of a crash) is not up to date. When the
  pipeline ends, all the metadata and progress data is saved
  immediately, regardless of `seconds_meta_append`.

  When the pipeline is just skipping targets, the actual interval
  between saves is `max(1, seconds_meta_append)` to reduce overhead.

- seconds_meta_upload:

  Positive numeric of length 1 with the minimum number of seconds
  between uploads of the metadata and progress data to the cloud (see
  <https://books.ropensci.org/targets/cloud-storage.html>). Higher
  values generally make the pipeline run faster, but unsaved work (in
  the event of a crash) may not be backed up to the cloud. When the
  pipeline ends, all the metadata and progress data is uploaded
  immediately, regardless of `seconds_meta_upload`.

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

  The `envir` argument of `tar_make()` and related functions always
  overrides the current value of `tar_option_get("envir")` in the
  current R session just before running the target script file, so
  whenever you need to set an alternative `envir`, you should always set
  it with
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

- garbage_collection:

  Deprecated. Use the `garbage_collection` argument of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  instead to run garbage collection at regular intervals in a pipeline,
  or use the argument of the same name in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  to activate garbage collection for a specific target.

- use_crew:

  Logical of length 1, whether to use `crew` if the `controller` option
  is set in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  in the target script (`_targets.R`). See
  <https://books.ropensci.org/targets/crew.html> for details.

- terminate_controller:

  Logical of length 1. For a `crew`-integrated pipeline, whether to
  terminate the controller after stopping or finishing the pipeline.
  This should almost always be set to `TRUE`, but `FALSE` combined with
  `callr_function = NULL` will allow you to get the running controller
  using `tar_option_get("controller")` for debugging purposes. For
  example, `tar_option_get("controller")$summary()` produces a
  worker-by-worker summary of the work assigned and completed,
  `tar_option_get("controller")$queue` is the list of unresolved tasks,
  and `tar_option_get("controller")$results` is the list of tasks that
  completed but were not collected with `pop()`. You can manually
  terminate the controller with `tar_option_get("controller")$summary()`
  to close down the dispatcher and worker processes.

- as_job:

  `TRUE` to run as an RStudio IDE / Posit Workbench job, if running on
  RStudio IDE / Posit Workbench. `FALSE` to run as a `callr` process in
  the main R session (depending on the `callr_function` argument). If
  `as_job` is `TRUE`, then the `rstudioapi` package must be installed.

## Value

`NULL` except if `callr_function = callr::r_bg()`, in which case a
handle to the `callr` background process is returned. Either way, the
value is invisibly returned.

## Storage access

Several functions like `tar_make()`,
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

Other pipeline:
[`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
[`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)

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
tar_make(starts_with("y")) # Only processes y1 and y2.
# Distributed computing with crew:
if (requireNamespace("crew", quietly = TRUE)) {
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(controller = crew::controller_local())
  list(
    tar_target(y1, 1 + 1),
    tar_target(y2, 1 + 1),
    tar_target(z, y1 + y2)
  )
}, ask = FALSE)
tar_make()
}
})
}
```
