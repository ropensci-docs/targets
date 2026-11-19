# Use targets

Set up `targets` for an existing project.

## Usage

``` r
use_targets(
  script = targets::tar_config_get("script"),
  open = interactive(),
  overwrite = FALSE,
  scheduler = NULL,
  job_name = NULL
)
```

## Arguments

- script:

  Character of length 1, where to write the target script file. Defaults
  to `tar_config_get("script")`, which in turn defaults to `_targets.R`.

- open:

  Logical of length 1, whether to open the file for editing in the
  RStudio IDE.

- overwrite:

  Logical of length 1, `TRUE` to overwrite the the target script file,
  `FALSE` otherwise.

- scheduler:

  Deprecated in `targets` version 1.5.0.9001 (2024-02-12).

- job_name:

  Deprecated in `targets` version 1.5.0.9001 (2024-02-12).

## Value

`NULL` (invisibly).

## Details

`use_targets()` writes an example `_targets.R` script to get started
with a `targets` pipeline for the current project. Follow the comments
in this script to adapt it as needed. For more information, please visit
<https://books.ropensci.org/targets/walkthrough.html>.

## See also

Other help:
[`tar_reprex()`](https://docs.ropensci.org/targets/reference/tar_reprex.md),
[`targets-package`](https://docs.ropensci.org/targets/reference/targets-package.md),
[`use_targets_rmd()`](https://docs.ropensci.org/targets/reference/use_targets_rmd.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
use_targets(open = FALSE)
})
}
```
