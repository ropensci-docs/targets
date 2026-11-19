# Produce a data frame of information about your targets.

Along with
[`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
and
[`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md),
`tar_manifest()` helps check that you constructed your pipeline
correctly.

## Usage

``` r
tar_manifest(
  names = NULL,
  fields = tidyselect::any_of(c("name", "command", "pattern", "description")),
  drop_missing = TRUE,
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function),
  envir = parent.frame(),
  script = targets::tar_config_get("script")
)
```

## Arguments

- names:

  Names of the targets to show. Set to `NULL` to show all the targets
  (default). Otherwise, the object supplied to `names` should be a
  `tidyselect` expression like
  [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html) or
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  from `tidyselect` itself, or
  [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  to select target names based on their descriptions.

- fields:

  Names of the fields, or columns, to show. Set to `NULL` to show all
  the fields (default). Otherwise, the value of `fields` should be a
  `tidyselect` expression like
  [`starts_with()`](https://tidyselect.r-lib.org/reference/starts_with.html)
  to select the columns to show in the output. Possible fields are
  below. All of them can be set in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md),
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md),
  or
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).

  - `name`: Name of the target.

  - `command`: the R command that runs when the target runs.

  - `description`: custom free-form text description of the target, if
    available.

  - `pattern`: branching pattern of the target, if applicable.

  - `format`: Storage format.

  - `repository`: Storage repository.

  - `iteration`: Iteration mode for branching.

  - `error`: Error mode, what to do when the target fails.

  - `memory`: Memory mode, when to keep targets in memory.

  - `storage`: Storage mode for high-performance computing scenarios.

  - `retrieval`: Retrieval mode for high-performance computing
    scenarios.

  - `deployment`: Where/whether to deploy the target in high-performance
    computing scenarios.

  - `priority`: Numeric of length 1 between 0 and 1. Controls which
    targets get deployed first when multiple competing targets are ready
    simultaneously. Targets with priorities closer to 1 get dispatched
    earlier (and polled earlier in
    [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)).

  - `resources`: A list of target-specific resource requirements for
    [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md).

  - `cue_mode`: Cue mode from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `cue_depend`: Depend cue from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `cue_expr`: Command cue from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `cue_file`: File cue from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `cue_format`: Format cue from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `cue_repository`: Repository cue from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `cue_iteration`: Iteration cue from
    [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md).

  - `packages`: List columns of packages loaded before running the
    target.

  - `library`: List column of library paths to load the packages.

- drop_missing:

  Logical of length 1, whether to automatically omit empty columns and
  columns with all missing values.

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

## Value

A data frame of information about the targets in the pipeline. Rows
appear in topological order (the order they will run without any
influence from parallel computing or priorities).

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

Other inspect:
[`tar_deps()`](https://docs.ropensci.org/targets/reference/tar_deps.md),
[`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md),
[`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md),
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
    tar_target(y2, 1 + 1),
    tar_target(z, y1 + y2),
    tar_target(m, z, pattern = map(z), description = "branching over z"),
    tar_target(c, z, pattern = cross(z))
  )
}, ask = FALSE)
tar_manifest()
tar_manifest(fields = any_of(c("name", "command")))
tar_manifest(fields = any_of("command"))
tar_manifest(fields = starts_with("cue"))
})
}
```
