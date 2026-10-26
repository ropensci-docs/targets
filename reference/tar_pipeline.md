# Declare a pipeline (deprecated).

Functions `tar_pipeline()` and
[`tar_bind()`](https://docs.ropensci.org/targets/reference/tar_bind.md)
are deprecated. Instead, simply end your target script file (default:
`_targets.R`) with a list of target definition objects. You can nest
these objects however you like.

## Usage

``` r
tar_pipeline(...)
```

## Arguments

- ...:

  Targets or lists of targets defined with
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## Value

A pipeline object.

## Details

Deprecated on 2021-01-03.

## Examples

``` r
# In _targets.R:
library(targets)
list( # You no longer need tar_pipeline() here.
  tar_target(data_file, "data.csv", format = "file"),
  list( # Target lists can be arbitrarily nested.
    tar_target(data_object, read.csv(data_file)),
    tar_target(analysis, analyze(data_object))
  )
)
#> [[1]]
#> <tar_stem> 
#>   name: data_file 
#>   description:  
#>   command:
#>     "data.csv" 
#>   format: file 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
#>   cue:
#>     seed: TRUE
#>     file: TRUE
#>     iteration: TRUE
#>     repository: TRUE
#>     format: TRUE
#>     depend: TRUE
#>     command: TRUE
#>     mode: thorough 
#>   packages:
#>     targets
#>     stats
#>     graphics
#>     grDevices
#>     utils
#>     datasets
#>     methods
#>     base 
#>   library:
#>     NULL
#> [[2]]
#> [[2]][[1]]
#> <tar_stem> 
#>   name: data_object 
#>   description:  
#>   command:
#>     read.csv(data_file) 
#>   format: rds 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
#>   cue:
#>     seed: TRUE
#>     file: TRUE
#>     iteration: TRUE
#>     repository: TRUE
#>     format: TRUE
#>     depend: TRUE
#>     command: TRUE
#>     mode: thorough 
#>   packages:
#>     targets
#>     stats
#>     graphics
#>     grDevices
#>     utils
#>     datasets
#>     methods
#>     base 
#>   library:
#>     NULL
#> [[2]][[2]]
#> <tar_stem> 
#>   name: analysis 
#>   description:  
#>   command:
#>     analyze(data_object) 
#>   format: rds 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
#>   cue:
#>     seed: TRUE
#>     file: TRUE
#>     iteration: TRUE
#>     repository: TRUE
#>     format: TRUE
#>     depend: TRUE
#>     command: TRUE
#>     mode: thorough 
#>   packages:
#>     targets
#>     stats
#>     graphics
#>     grDevices
#>     utils
#>     datasets
#>     methods
#>     base 
#>   library:
#>     NULL
#> 
```
