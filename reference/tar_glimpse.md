# Visualize an abridged fast dependency graph.

Analyze the pipeline defined in the target script file (default:
`_targets.R`) and visualize the directed acyclic graph of targets.
Unlike
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
`tar_glimpse()` does not account for metadata or progress information,
which means the graph renders faster. Also, `tar_glimpse()` omits
functions and other global objects by default (but you can include them
with `targets_only = FALSE`).

## Usage

``` r
tar_glimpse(
  targets_only = TRUE,
  names = NULL,
  shortcut = FALSE,
  allow = NULL,
  exclude = ".Random.seed",
  label = targets::tar_config_get("label"),
  label_length = targets::tar_config_get("label_length"),
  label_width = targets::tar_config_get("label_width"),
  level_separation = targets::tar_config_get("level_separation"),
  degree_from = 1L,
  degree_to = 1L,
  zoom_speed = 1,
  physics = FALSE,
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

- label:

  Character vector of one or more aesthetics to add to the vertex
  labels. Currently, the only option is `"description"` to show each
  target's custom description, or `character(0)` to suppress it.

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
[`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md),
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)

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
    tar_target(z, y1 + y2)
  )
}, ask = FALSE)
tar_glimpse()
tar_glimpse(allow = starts_with("y")) # see also any_of()
})
}
```
