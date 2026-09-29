# Shiny module UI for tar_watch()

Use `tar_watch_ui()` and
[`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md)
to include
[`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
as a Shiny module in an app.

## Usage

``` r
tar_watch_ui(
  id,
  label = "tar_watch_label",
  seconds = 10,
  seconds_min = 1,
  seconds_max = 60,
  seconds_step = 1,
  targets_only = FALSE,
  outdated = FALSE,
  label_tar_visnetwork = NULL,
  level_separation = 150,
  degree_from = 1L,
  degree_to = 1L,
  height = "650px",
  display = "summary",
  displays = c("summary", "branches", "progress", "graph", "about"),
  title = "",
  theme = bslib::bs_theme(),
  spinner = FALSE
)
```

## Arguments

- id:

  Character of length 1, ID corresponding to the UI function of the
  module.

- label:

  Label for the module.

- seconds:

  Numeric of length 1, default number of seconds between refreshes of
  the graph. Can be changed in the app controls.

- seconds_min:

  Numeric of length 1, lower bound of `seconds` in the app controls.

- seconds_max:

  Numeric of length 1, upper bound of `seconds` in the app controls.

- seconds_step:

  Numeric of length 1, step size of `seconds` in the app controls.

- targets_only:

  Logical, whether to restrict the output to just targets (`FALSE`) or
  to also include global functions and objects.

- outdated:

  Logical, whether to show colors to distinguish outdated targets from
  up-to-date targets. (Global functions and objects still show these
  colors.) Looking for outdated targets takes a lot of time for large
  pipelines with lots of branches, and setting `outdated` to `FALSE` is
  a nice way to speed up the graph if you only want to see dependency
  relationships and pipeline progress.

- label_tar_visnetwork:

  Character vector, `label` argument to
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md).

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

- height:

  Character of length 1, height of the `visNetwork` widget and branches
  table.

- display:

  Character of length 1, which display to show first.

- displays:

  Character vector of choices for the display. Elements can be any of
  `"graph"`, `"summary"`, `"branches"`, or `"about"`.

- title:

  Character of length 1, title of the UI.

- theme:

  A call to
  [`bslib::bs_theme()`](https://rstudio.github.io/bslib/reference/bs_theme.html)
  with the `bslib` theme.

- spinner:

  `TRUE` to add a busy spinner, `FALSE` to omit.

## Value

A Shiny module UI.

## See also

Other progress:
[`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md),
[`tar_completed()`](https://docs.ropensci.org/targets/reference/tar_completed.md),
[`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md),
[`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md),
[`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md),
[`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md),
[`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md),
[`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md),
[`tar_skipped()`](https://docs.ropensci.org/targets/reference/tar_skipped.md),
[`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
[`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md)
