# Code dependencies

List the dependencies of a function or expression. `tar_deps()` expects
the `expr` argument to be an unevaluated expression, whereas
`tar_deps_raw()` expects `expr` to be an evaluated expression object.
Functions can be passed normally in either case.

## Usage

``` r
tar_deps(expr)

tar_deps_raw(expr)
```

## Arguments

- expr:

  An R expression or function. `tar_deps()` expects the `expr` argument
  to be an unevaluated expression, whereas `tar_deps_raw()` expects
  `expr` to be an evaluated expression object. Functions can be passed
  normally in either case.

## Value

Character vector of the dependencies of a function or expression.

## Details

`targets` detects the dependencies of commands using static code
analysis. Use `tar_deps()` to run the code analysis and see the
dependencies for yourself.

## See also

[`tar_branches()`](https://docs.ropensci.org/targets/reference/tar_branches.md),
[`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md)

Other inspect:
[`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md),
[`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md),
[`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md),
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md),
[`tar_sitrep()`](https://docs.ropensci.org/targets/reference/tar_sitrep.md),
[`tar_validate()`](https://docs.ropensci.org/targets/reference/tar_validate.md)

## Examples

``` r
tar_deps(x <- y + z)
#> [1] "+"  "<-" "y"  "z" 
tar_deps(quote(x <- y + z))
#> [1] "quote"
tar_deps({
  x <- 1
  x + a
})
#> [1] "+"  "<-" "a"  "{" 
tar_deps(function(a = b) map_dfr(data, ~do_row(.x)))
#> [1] ".x"      "b"       "data"    "do_row"  "map_dfr"
tar_deps_raw(function(a = b) map_dfr(data, ~do_row(.x)))
#> [1] ".x"      "b"       "data"    "do_row"  "map_dfr"
```
