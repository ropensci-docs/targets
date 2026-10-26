# Shiny module server for tar_watch()

Use
[`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)
and `tar_watch_server()` to include
[`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
as a Shiny module in an app.

## Usage

``` r
tar_watch_server(
  id,
  height = "650px",
  exclude = ".Random.seed",
  config = Sys.getenv("TAR_CONFIG", "_targets.yaml"),
  project = Sys.getenv("TAR_PROJECT", "main")
)
```

## Arguments

- id:

  Character of length 1, ID corresponding to the UI function of the
  module.

- height:

  Character of length 1, height of the `visNetwork` widget and branches
  table.

- exclude:

  Character vector of nodes to omit from the graph.

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

## Value

A Shiny module server.

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
[`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)
