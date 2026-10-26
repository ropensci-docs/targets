# Interactive mode pipeline

Not a user-side function. Do not invoke directly. Only exported to on a
technicality.

## Usage

``` r
tar_make_interactive(code)
```

## Arguments

- code:

  Character vector of lines of a `_targets.R` file to define a pipeline.

## Value

`NULL` (invisibly).

## Examples

``` r
if (identical(Sys.getenv("TAR_INTERACTIVE_EXAMPLES"), "true")) {
tar_make_interactive("library(targets); tar_target(x, 123)")
message(x)
}
```
