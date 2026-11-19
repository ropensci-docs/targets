# Read `_targets.yaml`.

Read the YAML content of `_targets.yaml`.

## Usage

``` r
tar_config_yaml(config = Sys.getenv("TAR_CONFIG", "_targets.yaml"))
```

## Arguments

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

## Value

Nested list of fields defined in `_targets.yaml`.

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

## Configuration

For several key functions like
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
the default values of arguments are controlled though
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md).
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
retrieves data from an optional YAML configuration file. You can control
the settings in the YAML file programmatically with
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md).
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

## See also

Other configuration:
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
[`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md),
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md),
[`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md),
[`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md),
[`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
[`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md),
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
[`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md),
[`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)

## Examples

``` r
yaml <- tempfile()
tar_config_set(store = "my_store_a", config = yaml, project = "project_a")
tar_config_set(store = "my_store_b", config = yaml, project = "project_b")
str(tar_config_yaml(config = yaml))
#> List of 2
#>  $ project_a:List of 1
#>   ..$ store: chr "my_store_a"
#>  $ project_b:List of 1
#>   ..$ store: chr "my_store_b"
```
