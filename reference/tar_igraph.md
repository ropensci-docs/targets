# Get the igraph.

Return an igraph object with the dependency graph of the pipeline.

## Usage

``` r
tar_igraph(
  targets_only = FALSE,
  callr_function = callr::r,
  callr_arguments = targets::tar_callr_args_default(callr_function),
  envir = parent.frame(),
  script = targets::tar_config_get("script")
)
```

## Arguments

- targets_only:

  `TRUE` to restrict the output to just the actual targets in the
  pipeline, `FALSE` to include imported global functions and objects as
  well.

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

An `igraph` object with the dependency graph.

## Details

If you have `igraph` \>= 2.2.0 installed, then you can debug cycles in
your pipeline by running `igraph::find_cycle(tar_igraph())`. This helps
detect cycles in the dependency graph of the pipeline. A cycle happens
when a target directly or indirectly depends on itself, which is not
allowed.
[`igraph::find_cycle()`](https://r.igraph.org/reference/find_cycle.html)
returns a character vector of the names of the targets and objects
involved in the cycle (if a cycle exists).

## See also

Other inspect:
[`tar_deps()`](https://docs.ropensci.org/targets/reference/tar_deps.md),
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md),
[`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md),
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md),
[`tar_sitrep()`](https://docs.ropensci.org/targets/reference/tar_sitrep.md),
[`tar_validate()`](https://docs.ropensci.org/targets/reference/tar_validate.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
  tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
    tar_script(
      list(
        tar_target(x, 1),
        tar_target(y, 2)
      ),
      ask = FALSE
    )
    tar_igraph()
  })
}
```
