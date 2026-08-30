# Show the cue-by-cue status of each target.

For each target, report which cues are activated. Except for the `never`
cue, the target will rerun in
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
if any cue is activated. The target is suppressed if the `never` cue is
`TRUE`. See
[`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
for details.

## Usage

``` r
tar_sitrep(
  names = NULL,
  fields = NULL,
  shortcut = targets::tar_config_get("shortcut"),
  reporter = targets::tar_config_get("reporter_outdated"),
  seconds_reporter = targets::tar_config_get("seconds_reporter_outdated"),
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function, reporter),
  envir = parent.frame(),
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- names:

  Optional, names of the targets. If supplied, `tar_sitrep()` only
  returns metadata on these targets. The object supplied to `names`
  should be `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- fields:

  Optional, names of columns/fields to select. If supplied,
  `tar_sitrep()` only returns the selected metadata columns. You can
  supply symbols or `tidyselect` helpers like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) and
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).
  The `name` column is always included first no matter what you select.
  Choices:

  - `name`: name of the target or global object.

  - `meta`: Whether the `meta` cue is activated: `TRUE` if the target is
    not in the metadata
    ([`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md)),
    or if the target errored during the last
    [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
    or if the class of the target changed.

  - `always`: Whether `mode` in
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
    is `"always"`. If `TRUE`,
    [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
    always runs the target.

  - `never`: Whether `mode` in
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
    is `"never"`. If `TRUE`,
    [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
    will only run if the `meta` cue activates.

  - `command`: Whether the target's command changed since last time.
    Always `TRUE` if the `meta` cue is activated. Otherwise, always
    `FALSE` if the `command` cue is suppressed.

  - `depend`: Whether the data/output of at least one of the target's
    dependencies changed since last time. Dependencies are targets,
    functions, and global objects directly upstream. Call
    `tar_outdated(targets_only = FALSE)` or
    `tar_visnetwork(targets_only = FALSE)` to see exactly which
    dependencies are outdated. Always `NA` if the `meta` cue is
    activated. Otherwise, always `FALSE` if the `depend` cue is
    suppressed.

  - `format`: Whether the storage format of the target is different from
    last time. Always `NA` if the `meta` cue is activated. Otherwise,
    always `FALSE` if the `format` cue is suppressed.

  - `repository`: Whether the storage repository of the target is
    different from last time. Always `NA` if the `meta` cue is
    activated. Otherwise, always `FALSE` if the `format` cue is
    suppressed.

  - `iteration`: Whether the iteration mode of the target is different
    from last time. Always `NA` if the `meta` cue is activated.
    Otherwise, always `FALSE` if the `iteration` cue is suppressed.

  - `file`: Whether the file(s) with the target's return value are
    missing or different from last time. Always `NA` if the `meta` cue
    is activated. Otherwise, always `FALSE` if the `file` cue is
    suppressed.

- shortcut:

  Logical of length 1, how to interpret the `names` argument. If
  `shortcut` is `FALSE` (default) then the function checks all targets
  upstream of `names` as far back as the dependency graph goes. If
  `TRUE`, then the function only checks the targets in `names` and uses
  stored metadata for information about upstream dependencies as needed.
  `shortcut = TRUE` increases speed if there are a lot of up-to-date
  targets, but it assumes all the dependencies are up to date, so please
  use with caution. Use with caution. `shortcut = TRUE` only works if
  you set `names`.

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

A data frame with one row per target/object and one column per cue. Each
element is a logical to indicate whether the cue is activated for the
target. See the `field` argument in this help file for details.

## Details

Caveats:

- [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
  allows you to change/suppress cues, so the return value will depend on
  the settings you supply to
  [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

- If a pattern tries to branches over a target that does not exist in
  storage, then the branches are omitted from the output.

- `tar_sitrep()` is myopic. It only considers what happens to the
  immediate target and its immediate upstream dependencies, and it makes
  no attempt to propagate invalidation downstream.

## See also

Other inspect:
[`tar_deps()`](https://docs.ropensci.org/targets/reference/tar_deps.md),
[`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md),
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md),
[`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md),
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md),
[`tar_validate()`](https://docs.ropensci.org/targets/reference/tar_validate.md)

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
tar_sitrep()
tar_meta(starts_with("y_")) # see also any_of()
})
}
```
