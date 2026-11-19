# Set configuration settings.

`tar_config_set()` writes special custom settings for the current
project to an optional YAML configuration file.

## Usage

``` r
tar_config_set(
  inherits = NULL,
  as_job = NULL,
  garbage_collection = NULL,
  label = NULL,
  label_length = NULL,
  label_width = NULL,
  level_separation = NULL,
  reporter_make = NULL,
  reporter_outdated = NULL,
  script = NULL,
  seconds_meta_append = NULL,
  seconds_meta_upload = NULL,
  seconds_reporter = NULL,
  seconds_reporter_outdated = NULL,
  seconds_interval = NULL,
  store = NULL,
  shortcut = NULL,
  use_crew = NULL,
  workers = NULL,
  config = Sys.getenv("TAR_CONFIG", "_targets.yaml"),
  project = Sys.getenv("TAR_PROJECT", "main")
)
```

## Arguments

- inherits:

  Character of length 1, name of the project from which the current
  project should inherit configuration settings. The current project is
  the `project` argument, which defaults to
  `Sys.getenv("TAR_PROJECT", "main")`. If the `inherits` argument
  `NULL`, the `inherits` setting is not modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- as_job:

  Logical of length 1, `as_job` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
  `TRUE` to run as an RStudio IDE / Posit Workbench job, `FALSE` to run
  as a `callr` process in the main R session (depending on the
  `callr_function` argument). If `as_job_` is `TRUE`, then the
  `rstudioapi` package must be installed.

- garbage_collection:

  Deprecated. Use the `garbage_collection` argument of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  instead to run garbage collection at regular intervals in a pipeline,
  or use the argument of the same name in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  to activate garbage collection for a specific target.

- label:

  Argument of
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  and
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  to control node labels.

- label_length:

  Argument of
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  and
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  to control the maximum number of content characters of each node
  label.

- label_width:

  Argument of
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  and
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  to control the maximum width of the node labels.

- level_separation:

  Argument of
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  and
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  to control the space between hierarchical levels.

- reporter_make:

  Character of length 1, `reporter` argument to
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and related functions that run the pipeline. If the argument `NULL`,
  the setting is not modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- reporter_outdated:

  Character of length 1, `reporter` argument to
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
  and related functions that do not run the pipeline. If the argument
  `NULL`, the setting is not modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- script:

  Character of length 1, path to the target script file that defines the
  pipeline (`_targets.R` by default). This path should be either an
  absolute path or a path relative to the project root where you will
  call
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and other functions. When
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and friends run the script from the current working directory. If the
  argument `NULL`, the setting is not modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- seconds_meta_append:

  Argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md).
  Positive numeric of length 1 with the minimum number of seconds
  between saves to the local metadata and progress files in the data
  store. This is an aggressive optimization setting not recommended for
  most users: higher values might make the pipeline run faster, but
  unsaved work (in the event of a crash) is not up to date.

  When the pipeline ends, all the metadata and progress data is saved
  immediately, regardless of `seconds_meta_append`. When the pipeline is
  just skipping targets, the actual interval between saves is
  `max(1, seconds_meta_append)` to reduce overhead.

- seconds_meta_upload:

  Argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md).
  Positive numeric of length 1 with the minimum number of seconds
  between uploads of the metadata and progress data to the cloud (see
  <https://books.ropensci.org/targets/cloud-storage.html>). Higher
  values generally make the pipeline run faster, but unsaved work (in
  the event of a crash) may not be backed up to the cloud. When the
  pipeline ends, all the metadata and progress data is uploaded
  immediately, regardless of `seconds_meta_upload`.

- seconds_reporter:

  Deprecated in `targets` 1.10.1.9010 (2025-03-31).

- seconds_reporter_outdated:

  Deprecated in `targets` 1.10.1.9010 (2025-03-31).

- seconds_interval:

  Deprecated on 2023-08-24 (`targets` version 1.2.2.9001). Use
  `seconds_meta_append`, `seconds_meta_upload`, and `seconds_reporter`
  instead.

- store:

  Character of length 1, path to the data store of the pipeline. If
  `NULL`, the `store` setting is left unchanged in the YAML
  configuration file (default: `_targets.yaml`). Usually, the data store
  lives at `_targets`. Set `store` to a custom directory to specify a
  path other than `_targets/`. The path need not exist before the
  pipeline begins, and it need not end with "\_targets", but it must be
  writeable. For optimal performance, choose a storage location with
  fast read/write access. If the argument `NULL`, the setting is not
  modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- shortcut:

  logical of length 1, default `shortcut` argument to
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and related functions. If the argument `NULL`, the setting is not
  modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- use_crew:

  Logical of length 1, whether to use `crew` in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  if the `controller` option is set in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  in the target script (`_targets.R`). See
  <https://books.ropensci.org/targets/crew.html> for details.

- workers:

  Positive numeric of length 1, `workers` argument of
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  and related functions that run the pipeline with parallel computing
  among targets. If the argument `NULL`, the setting is not modified.
  Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  to delete a setting.

- config:

  Character of length 1, file path of the YAML configuration file with
  `targets` project settings. The `config` argument specifies which YAML
  configuration file that
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  reads from or `tar_config_set()` writes to in a single function call.
  It does not globally change which configuration file is used in
  subsequent function calls. The default file path of the YAML file is
  always `_targets.yaml` unless you set another default path using the
  `TAR_CONFIG` environment variable, e.g.
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
  call to `tar_config_set()` or
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).
  The default project is always called `"main"` unless you set another
  default project using the `TAR_PROJECT` environment variable, e.g.
  `Sys.setenv(tar_project = "custom")`. This also has the effect of
  temporarily modifying the default arguments to other functions such as
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  because the default arguments to those functions are controlled by
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).

## Value

`NULL` (invisibly)

## Configuration

For several key functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
the default values of arguments are controlled though
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
retrieves data from an optional YAML configuration file. You can control
the settings in the YAML file programmatically with `tar_config_set()`.
The default file path of this YAML file is `_targets.yaml`, and you can
set another path globally using the `TAR_CONFIG` environment variable.
The YAML file can store configuration settings for multiple projects,
and you can globally set the default project with the `TAR_PROJECT`
environment variable. The structure of the YAML file follows rules
similar to the `config` R package, e.g. projects can inherit settings
from one another using the `inherits` field. Exceptions include:

1.  There is no requirement to have a configuration named `"default"`.

2.  Other projects do not inherit from the default project\`
    automatically.

3.  Not all fields need values because `targets` already has defaults.

`targets` does not actually invoke the `config` package. The
implementation in `targets` was written from scratch without viewing or
copying any part of the source code of `config`.

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

Other configuration:
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
[`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md),
[`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md),
[`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md),
[`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md),
[`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
[`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md),
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
[`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md),
[`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(list(tar_target(x, 1 + 1)))
tar_config_get("store") # NULL (data store defaults to "_targets/")
store_path <- tempfile()
tar_config_set(store = store_path)
tar_config_get("store") # Shows a temp file.
tar_make() # Writes to the custom data store identified in _targets.yaml.
tar_read(x) # tar_read() knows about _targets.yaml too.
file.exists("_targets") # FALSE
file.exists(store_path) # TRUE
})
}
```
