# For developers only: get the definition of the current target.

For developers only: get the full definition of the target currently
running. This target definition is the same kind of object produced by
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## Usage

``` r
tar_definition(
  default = targets::tar_target_raw("target_name", quote(identity()))
)
```

## Arguments

- default:

  Environment, value to return if `tar_definition()` is called on its
  own outside a `targets` pipeline. Having a default lets users run
  things without
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  which helps peel back layers of code and troubleshoot bugs.

## Value

If called from a running target, `tar_definition()` returns the target
definition object of the currently running target. See the "Target
definition objects" section for details.

## Details

Most users should not use `tar_definition()` because accidental
modifications could break the pipeline. `tar_definition()` only exists
in order to support third-party interface packages, and even then the
returned target definition is not modified..

## Target definition objects

Functions like
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
produce target definition objects, special objects with specialized sets
of S3 classes. Target definition objects represent skippable steps of
the analysis pipeline as described at
<https://books.ropensci.org/targets/>. Please read the walkthrough at
<https://books.ropensci.org/targets/walkthrough.html> to understand the
role of target definition objects in analysis pipelines.

For developers,
<https://wlandau.github.io/targetopia/contributing.html#target-factories>
explains target factories (functions like this one which generate
targets) and the design specification at
<https://books.ropensci.org/targets-design/> details the structure and
composition of target definition objects.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
[`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md),
[`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md),
[`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md),
[`tar_envir()`](https://docs.ropensci.org/targets/reference/tar_envir.md),
[`tar_format_get()`](https://docs.ropensci.org/targets/reference/tar_format_get.md),
[`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.md),
[`tar_name()`](https://docs.ropensci.org/targets/reference/tar_name.md),
[`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md),
[`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md),
[`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md),
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md),
[`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)

## Examples

``` r
class(tar_definition())
#> [1] "tar_stem"    "tar_builder" "tar_target"  "environment"
tar_definition()$name
#> [1] "target_name"
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(
  tar_target(x, tar_definition()$settings$memory, memory = "transient")
)
tar_make(x)
tar_read(x)
})
}
```
