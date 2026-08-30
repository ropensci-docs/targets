# Emulate dynamic branching.

Emulate the dynamic branching process outside a pipeline.
`tar_pattern()` can help you understand the overall branching structure
that comes from the `pattern` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## Usage

``` r
tar_pattern(pattern, ..., seed = 0L)
```

## Arguments

- pattern:

  Function call with the pattern specification.

- ...:

  Named integers, each of length 1. Each name is the name of a
  dependency target, and each integer is the length of the target
  (number of branches or slices). Names must be unique.

- seed:

  Integer of length 1, random number generator seed to emulate the
  pattern reproducibly. (The `sample()` pattern is random). In a real
  pipeline, the seed is automatically generated from the target name in
  deterministic fashion.

## Value

A `tibble` showing the kinds of dynamic branches that
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
would create in a real pipeline with the given `pattern`. Each row is a
dynamic branch, each column is a dependency target, and each element is
the name of an upstream bud or branch that the downstream branch depends
on. Buds are pieces of non-branching targets ("stems") and branches are
pieces of patterns. The returned bud and branch names are not the actual
ones you will see when you run the pipeline, but they do communicate the
branching structure of the pattern.

## Details

Dynamic branching is a way to programmatically create multiple new
targets based on the values of other targets, all while the pipeline is
running. Use the `pattern` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
to get started. `pattern` accepts a function call composed of target
names and any of the following patterns:

- `map()`: iterate over one or more targets in sequence.

- `cross()`: iterate over combinations of slices of targets.

- [`slice()`](https://dplyr.tidyverse.org/reference/slice.html): select
  one or more slices by index, e.g. `slice(x, index = c(3, 4))` selects
  the third and fourth slice or branch of `x`.

- `head()`: restrict branching to the first few elements.

- `tail()`: restrict branching to the last few elements.

- `sample()`: restrict branching to a random subset of elements.

## See also

Other branching:
[`tar_branch_index()`](https://docs.ropensci.org/targets/reference/tar_branch_index.md),
[`tar_branch_names()`](https://docs.ropensci.org/targets/reference/tar_branch_names.md),
[`tar_branches()`](https://docs.ropensci.org/targets/reference/tar_branches.md)

## Examples

``` r
# To use dynamic map for real in a pipeline,
# call map() in a target's pattern.
# The following code goes at the bottom of
# your target script file (default: `_targets.R`).
list(
  tar_target(x, seq_len(2)),
  tar_target(y, head(letters, 2)),
  tar_target(dynamic, c(x, y), pattern = map(x, y)) # 2 branches
)
#> [[1]]
#> <tar_stem> 
#>   name: x 
#>   description:  
#>   command:
#>     seq_len(2) 
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
#> [[2]]
#> <tar_stem> 
#>   name: y 
#>   description:  
#>   command:
#>     head(letters, 2) 
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
#> [[3]]
#> <tar_pattern> 
#>   name: dynamic 
#>   description:  
#>   command:
#>     c(x, y) 
#>   pattern:
#>     map(x, y) 
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
# Likewise for more complicated patterns.
list(
  tar_target(x, seq_len(2)),
  tar_target(y, head(letters, 2)),
  tar_target(z, head(LETTERS, 2)),
  tar_target(dynamic, c(x, y, z), pattern = cross(z, map(x, y))) #4 branches
)
#> [[1]]
#> <tar_stem> 
#>   name: x 
#>   description:  
#>   command:
#>     seq_len(2) 
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
#> [[2]]
#> <tar_stem> 
#>   name: y 
#>   description:  
#>   command:
#>     head(letters, 2) 
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
#> [[3]]
#> <tar_stem> 
#>   name: z 
#>   description:  
#>   command:
#>     head(LETTERS, 2) 
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
#> [[4]]
#> <tar_pattern> 
#>   name: dynamic 
#>   description:  
#>   command:
#>     c(x, y, z) 
#>   pattern:
#>     cross(z, map(x, y)) 
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
# But you can emulate dynamic branching without running a pipeline
# in order to understand the patterns you are creating. Simply supply
# the pattern and the length of each dependency target.
# The returned data frame represents the branching structure of the pattern:
# One row per new branch, one column per dependency target, and
# one element per bud/branch in each dependency target.
tar_pattern(
  cross(x, map(y, z)),
  x = 2,
  y = 3,
  z = 3
)
#> # A tibble: 6 × 3
#>   x     y     z    
#>   <chr> <chr> <chr>
#> 1 x_1   y_1   z_1  
#> 2 x_1   y_2   z_2  
#> 3 x_1   y_3   z_3  
#> 4 x_2   y_1   z_1  
#> 5 x_2   y_2   z_2  
#> 6 x_2   y_3   z_3  
tar_pattern(
  head(cross(x, map(y, z)), n = 2),
  x = 2,
  y = 3,
  z = 3
)
#> # A tibble: 2 × 3
#>   x     y     z    
#>   <chr> <chr> <chr>
#> 1 x_1   y_1   z_1  
#> 2 x_1   y_2   z_2  
```
