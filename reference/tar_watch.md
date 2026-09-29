# Shiny app to watch the dependency graph.

Launches a background process with a Shiny app that calls
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
every few seconds. To embed this app in other apps, use the Shiny module
in
[`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)
and
[`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md).

## Usage

``` r
tar_watch(
  seconds = 10,
  seconds_min = 1,
  seconds_max = 60,
  seconds_step = 1,
  targets_only = FALSE,
  exclude = ".Random.seed",
  outdated = FALSE,
  label = NULL,
  level_separation = 150,
  degree_from = 1L,
  degree_to = 1L,
  config = Sys.getenv("TAR_CONFIG", "_targets.yaml"),
  project = Sys.getenv("TAR_PROJECT", "main"),
  height = "650px",
  display = "summary",
  displays = c("summary", "branches", "progress", "graph", "about"),
  background = TRUE,
  browse = TRUE,
  host = getOption("shiny.host", "127.0.0.1"),
  port = getOption("shiny.port", targets::tar_random_port()),
  verbose = TRUE,
  supervise = TRUE,
  poll_connection = TRUE,
  stdout = "|",
  stderr = "|",
  title = "",
  theme = bslib::bs_theme(),
  spinner = TRUE
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

- exclude:

  Character vector of nodes to omit from the graph.

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

- config:

  Character of length 1, file path of the YAML configuration file with
  `targets` project settings. The `config` argument specifies which YAML
  configuration file that
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  reads from or
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  writes to in a single function call. It does not globally change which
  configuration file is used in subsequent function calls. The default
  file path of the YAML file is always `_targets.yaml` unless you set
  another default path using the `TAR_CONFIG` environment variable, e.g.
  `Sys.setenv(TAR_CONFIG = "custom.yaml")`. This also has the effect of
  temporarily modifying the default arguments to other functions such as
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  because the default arguments to those functions are controlled by
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).

- project:

  Character of length 1, name of the current `targets` project. Thanks
  to the `config` R package, `targets` YAML configuration files can
  store multiple sets of configuration settings, with each set
  corresponding to its own project. The `project` argument allows you to
  set or get a configuration setting for a specific project for a given
  call to
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  or
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).
  The default project is always called `"main"` unless you set another
  default project using the `TAR_PROJECT` environment variable, e.g.
  `Sys.setenv(tar_project = "custom")`. This also has the effect of
  temporarily modifying the default arguments to other functions such as
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  because the default arguments to those functions are controlled by
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).

- height:

  Character of length 1, height of the `visNetwork` widget and branches
  table.

- display:

  Character of length 1, which display to show first.

- displays:

  Character vector of choices for the display. Elements can be any of
  `"graph"`, `"summary"`, `"branches"`, or `"about"`.

- background:

  Logical, whether to run the app in a background process so you can
  still use the R console while the app is running.

- browse:

  Whether to open the app in a browser when the app is ready. Only
  relevant if `background` is `TRUE`.

- host:

  Character of length 1, IPv4 address to listen on. Only relevant if
  `background` is `TRUE`.

- port:

  Positive integer of length 1, TCP port to listen on. Only relevant if
  `background` is `TRUE`.

- verbose:

  whether to print a spinner and informative messages. Only relevant if
  `background` is `TRUE`.

- supervise:

  Whether to register the process with a supervisor. If `TRUE`, the
  supervisor will ensure that the process is killed when the R process
  exits.

- poll_connection:

  Whether to have a control connection to the process. This is used to
  transmit messages from the subprocess to the main process.

- stdout:

  The name of the file the standard output of the child R process will
  be written to. If the child process runs with the `--slave` option
  (the default), then the commands are not echoed and will not be shown
  in the standard output. Also note that you need to call
  [`print()`](https://rdrr.io/r/base/print.html) explicitly to show the
  output of the command(s). IF `NULL` (the default), then standard
  output is not returned, but it is recorded and included in the error
  object if an error happens.

- stderr:

  The name of the file the standard error of the child R process will be
  written to. In particular
  [`message()`](https://rdrr.io/r/base/message.html) sends output to the
  standard error. If nothing was sent to the standard error, then this
  file will be empty. This argument can be the same file as `stdout`, in
  which case they will be correctly interleaved. If this is the string
  `"2>&1"`, then standard error is redirected to standard output. IF
  `NULL` (the default), then standard output is not returned, but it is
  recorded and included in the error object if an error happens.

- title:

  Character of length 1, title of the UI.

- theme:

  A call to
  [`bslib::bs_theme()`](https://rstudio.github.io/bslib/reference/bs_theme.html)
  with the `bslib` theme.

- spinner:

  `TRUE` to add a busy spinner, `FALSE` to omit.

## Value

A handle to
[`callr::r_bg()`](https://callr.r-lib.org/reference/r_bg.html)
background process running the app.

## Details

The controls of the app are in the left panel. The `seconds` control is
the number of seconds between refreshes of the graph, and the other
settings match the arguments of
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md).

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
[`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md),
[`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  sleep_run <- function(...) {
    Sys.sleep(10)
  }
  list(
    tar_target(settings, sleep_run()),
    tar_target(data1, sleep_run(settings)),
    tar_target(data2, sleep_run(settings))
  )
}, ask = FALSE)
# Launch the app in a background process.
tar_watch(seconds = 10, outdated = FALSE, targets_only = TRUE)
# Run the pipeline.
tar_make()
})
}
```
