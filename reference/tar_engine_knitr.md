# Target Markdown `knitr` engine

`knitr` language engine that runs `targets` code chunks in Target
Markdown.

## Usage

``` r
tar_engine_knitr(options)
```

## Arguments

- options:

  A named list of `knitr` chunk options.

## Value

Character, output generated from
[`knitr::engine_output()`](https://rdrr.io/pkg/knitr/man/engine_output.html).

## Target Markdown interactive mode

Target Markdown has two modes:

1.  Non-interactive mode. This is the default when you run
    [`knitr::knit()`](https://rdrr.io/pkg/knitr/man/knit.html) or
    [`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).
    Here, the code in `targets` code chunks gets written to special
    script files in order to set up a `targets` pipeline to run later.

2.  Interactive mode: here, no scripts are written to set up a pipeline.
    Rather, the globals or targets in question are run in the current
    environment and the values are assigned to that environment.

The mode is interactive if `!isTRUE(getOption("knitr.in.progress"))`, is
`TRUE`. The `knitr.in.progress` option is `TRUE` when you run
[`knitr::knit()`](https://rdrr.io/pkg/knitr/man/knit.html) or
[`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html)
and `NULL` if you are running one chunk at a time interactively in an
integrated development environment, e.g. the notebook interface in
RStudio: <https://bookdown.org/yihui/rmarkdown/notebook.html>. You can
choose the mode with the `tar_interactive` chunk option. (In `targets`
0.6.0, `tar_interactive` defaults to
[`interactive()`](https://rdrr.io/r/base/interactive.html) instead of
`!isTRUE(getOption("knitr.in.progress"))`.)

## Target Markdown chunk options

Target Markdown introduces the following `knitr` code chunk options.
Most other standard `knitr` code chunk options should just work in
non-interactive mode. In interactive mode, not all

- `tar_globals`: Logical of length 1, whether to define globals or
  targets. If `TRUE`, the chunk code defines functions, objects, and
  options common to all the targets. If `FALSE` or `NULL` (default),
  then the chunk returns formal targets for the pipeline.

- `tar_interactive`: Logical of length 1, whether to run in interactive
  mode or non-interactive mode. See the "Target Markdown interactive
  mode" section of this help file for details.

- `tar_name`: name to use for writing helper script files (e.g.
  `_targets_r/targets/target_script.R`) and specifying target names if
  the `tar_simple` chunk option is `TRUE`. All helper scripts and target
  names must have unique names, so please do not set this option
  globally with `knitr::opts_chunk$set()`.

- `tar_script`: Character of length 1, where to write the target script
  file in non-interactive mode. Most users can skip this option and
  stick with the default `_targets.R` script path. Helper script files
  are always written next to the target script in a folder with an
  `"_r"` suffix. The `tar_script` path must either be absolute or be
  relative to the project root (where you call
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  or similar). If not specified, the target script path defaults to
  `tar_config_get("script")` (default: `_targets.R`; helpers default:
  `_targets_r/`). When you run
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  etc. with a non-default target script, you must select the correct
  target script file either with the `script` argument or with
  `tar_config_set(script = ...)`. The function will
  [`source()`](https://rdrr.io/r/base/source.html) the script file from
  the current working directory (i.e. with `chdir = FALSE` in
  [`source()`](https://rdrr.io/r/base/source.html)).

- `tar_simple`: Logical of length 1. Set to `TRUE` to define a single
  target with a simplified interface. In code chunks with `tar_simple`
  equal to `TRUE`, the chunk label (or the `tar_name` chunk option if
  you set it) becomes the name, and the chunk code becomes the command.
  In other words, a code chunk with label `targetname` and command
  `mycommand()` automatically gets converted to
  `tar_target(name = targetname, command = mycommand())`. All other
  arguments of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  remain at their default values (configurable with
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  in a `tar_globals = TRUE` chunk).

## See also

<https://books.ropensci.org/targets/literate-programming.html>

Other Target Markdown:
[`tar_interactive()`](https://docs.ropensci.org/targets/reference/tar_interactive.md),
[`tar_noninteractive()`](https://docs.ropensci.org/targets/reference/tar_noninteractive.md),
[`tar_toggle()`](https://docs.ropensci.org/targets/reference/tar_toggle.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
# Register the engine.
if (requireNamespace("knitr", quietly = TRUE)) {
  knitr::knit_engines$set(targets = targets::tar_engine_knitr)
}
# Then, `targets` code chunks in a knitr report will run
# as described at
# <https://books.ropensci.org/targets/literate-programming.html>.
}
```
