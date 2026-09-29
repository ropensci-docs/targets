# visNetwork dependency graph.

Visualize the pipeline dependency graph with a `visNetwork` HTML widget.

## Usage

``` r
tar_visnetwork(
  targets_only = FALSE,
  names = NULL,
  shortcut = FALSE,
  allow = NULL,
  exclude = ".Random.seed",
  outdated = TRUE,
  label = targets::tar_config_get("label"),
  label_length = targets::tar_config_get("label_length"),
  label_width = targets::tar_config_get("label_width"),
  level_separation = targets::tar_config_get("level_separation"),
  degree_from = 1L,
  degree_to = 1L,
  zoom_speed = 1,
  physics = FALSE,
  reporter = targets::tar_config_get("reporter_outdated"),
  seconds_reporter = targets::tar_config_get("seconds_reporter_outdated"),
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function),
  envir = parent.frame(),
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store")
)
```

## Arguments

- targets_only:

  Logical, whether to restrict the output to just targets (`FALSE`) or
  to also include global functions and objects.

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

- label:

  Character vector of one or more aesthetics to add to the vertex
  labels. Can contain `"description"` to show each target's custom
  description, `"time"` to show total runtime, `"size"` to show total
  storage size, or `"branches"` to show the number of branches in each
  pattern. You can choose multiple aesthetics at once, e.g.
  `label = c("description", "time")`. Only the description is enabled by
  default.

- label_length:

  Positive numeric of length 1, maximum number of content characters of
  each node label.

- label_width:

  Positive numeric of length 1, maximum width of each node label.

- level_separation:

  Numeric of length 1, `levelSeparation` argument of
  [`visNetwork::visHierarchicalLayout()`](https://rdrr.io/pkg/visNetwork/man/visHierarchicalLayout.html).
  Controls the distance between hierarchical levels. Consider changing
  the value if the aspect ratio of the graph is far from 1. If
  `level_separation` is `NULL`, the `levelSeparation` argument of
  `visHierarchicalLayout()` defaults to a value chosen by `targets`.

- degree_from:

  Integer of length 1. When you click on a node, the graph highlights a
  neighborhood of that node. `degree_from` controls the number of edges
  the neighborhood extends upstream.

- degree_to:

  Integer of length 1. When you click on a node, the graph highlights a
  neighborhood of that node. `degree_to` controls the number of edges
  the neighborhood extends downstream.

- zoom_speed:

  Positive numeric of length 1, scaling factor on the zoom speed. Above
  1 zooms faster than default, below 1 zooms lower than default.

- physics:

  Logical of length 1, whether to implement interactive physics in the
  graph, e.g. edge elasticity.

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

A `visNetwork` HTML widget object.

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

Other visualize:
[`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md),
[`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set()
  list(
    tar_target(y1, 1 + 1),
    tar_target(y2, 1 + 1),
    tar_target(z, y1 + y2, description = "sum of two other sums")
  )
})
tar_visnetwork()
tar_visnetwork(allow = starts_with("y")) # see also any_of()
})
}
```
