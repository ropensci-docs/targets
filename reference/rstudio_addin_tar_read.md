# RStudio addin to call [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md) on the symbol at the cursor.

For internal use only. Not a user-side function.

## Usage

``` r
rstudio_addin_tar_read(context = NULL)
```

## Arguments

- context:

  RStudio API context from
  [`rstudioapi::getActiveDocumentContext()`](https://rstudio.github.io/rstudioapi/reference/rstudio-editors.html).
