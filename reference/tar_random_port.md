# Random TCP port

Not a user-side function. Exported for infrastructure purposes only.

## Usage

``` r
tar_random_port(lower = 49152L, upper = 65355L)
```

## Arguments

- lower:

  Integer of length 1, lowest possible port.

- upper:

  Integer of length 1, highest possible port.

## Value

A random port not likely to be used by another process.

## Examples

``` r
if (requireNamespace("parallelly", quietly = TRUE)) {
tar_random_port()
}
#> [1] 60921
```
