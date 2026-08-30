# Return the vertices and edges of a pipeline dependency graph.

Analyze the pipeline defined in the target script file (default:
`_targets.R`) and return the vertices and edges of the directed acyclic
graph of dependency relationships.

## Usage

``` r
tar_network(
  targets_only = FALSE,
  names = NULL,
  shortcut = FALSE,
  allow = NULL,
  exclude = NULL,
  outdated = TRUE,
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

- targets_only:

  `TRUE` to restrict the output to just the actual targets in the
  pipeline, `FALSE` to include imported global functions and objects as
  well.

- names:

  Names of targets. The graph visualization will operate only on these
  targets (and unless `shortcut` is `TRUE`, all the targets upstream as
  well). Selecting a small subgraph using `names` could speed up the
  load time of the visualization. Unlike `allow`, `names` is invoked
  before the graph is generated. Set to NULL to check/run all the
  targets (default). Otherwise, the object supplied to `names` should be
  a `tidyselect` expression like
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

- allow:

  Optional, the set of allowable vertices in the graph. Supplied as
  `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

  Unlike `names`, `allow` is invoked only after the graph is mostly
  resolved, so it will not speed up execution. Set to `NULL` to allow
  all vertices in the pipeline and environment (default).

- exclude:

  Optional, the set of exclude vertices from the graph. Supplied as
  `NULL` or a `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html).

  Unlike `names`, `exclude` is invoked only after the graph is mostly
  resolved, so it will not speed up execution.

  Unlike all other `tidyselect`-powered arguments in `targets`,
  `exclude` lets you supply target names that do not necessarily exist
  in the graph.

  Set `exclude` to `NULL` to exclude no vertices.

- outdated:

  Logical, whether to show colors to distinguish outdated targets from
  up-to-date targets. (Global functions and objects still show these
  colors.) Looking for outdated targets takes a lot of time for large
  pipelines with lots of branches, and setting `outdated` to `FALSE` is
  a nice way to speed up the graph if you only want to see dependency
  relationships and pipeline progress.

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

A list with two data frames: `vertices` and `edges`. The vertices data
frame has one row per target and columns with the the type of the target
or object (stem, branch, map, cross, function, or object), each target's
description, and each target's status (up to date, outdated, dispatched,
completed, canceled, or errored), as well as metadata if available
(seconds of runtime, bytes of storage, and number of dynamic branches).
The edges data frame has one row for every edge and columns `to` and
`from` to mark the starting and terminating vertices.

## Dependency graph

The dependency graph of a pipeline is a directed acyclic graph (DAG)
where each node indicates a target or global object and each directed
edge indicates where a downstream node depends on an upstream node. The
DAG is not always a tree, but it never contains a cycle because no
target is allowed to directly or indirectly depend on itself. The
dependency graph should show a natural progression of work from left to
right. `targets` uses static code analysis to create the graph, so the
order of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
calls in the `_targets.R` file does not matter. However, targets does
not support self-referential loops or other cycles. For more information
on the dependency graph, please read
<https://books.ropensci.org/targets/targets.html#dependencies>.

## See also

Other inspect:
[`tar_deps()`](https://docs.ropensci.org/targets/reference/tar_deps.md),
[`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md),
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md),
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md),
[`tar_sitrep()`](https://docs.ropensci.org/targets/reference/tar_sitrep.md),
[`tar_validate()`](https://docs.ropensci.org/targets/reference/tar_validate.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set()
  list(
    tar_target(y1, 1 + 1),
    tar_target(y2, 1 + 1, description = "y2 info"),
    tar_target(z, y1 + y2, description = "z info")
  )
}, ask = FALSE)
tar_network(targets_only = TRUE)
})
}
```
