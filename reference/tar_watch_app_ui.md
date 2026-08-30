# Create the full [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md) app UI.

Only exported for infrastructure purposes. Not a user-side function.
Users should instead call
[`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
directly.

## Usage

``` r
tar_watch_app_ui(
  seconds,
  seconds_min,
  seconds_max,
  seconds_step,
  targets_only,
  outdated,
  label,
  level_separation,
  degree_from,
  degree_to,
  height,
  display,
  displays,
  title,
  theme,
  spinner
)
```

## Arguments

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

- label:

  Label argument to
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

A Shiny UI.
