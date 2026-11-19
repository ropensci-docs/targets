# Set a seed to run a target.

`targets` generates its own target-specific seeds using
[`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md).
Use `tar_seed_set()` to set one of these seeds in R.

## Usage

``` r
tar_seed_set(seed)
```

## Arguments

- seed:

  Integer of length 1, value of the seed to set with
  [`set.seed()`](https://rdrr.io/r/base/Random.html).

## Value

`NULL` (invisibly).

## Details

`tar_seed_set()` gives the user-supplied `seed` to
[`set.seed()`](https://rdrr.io/r/base/Random.html) and sets arguments
`kind = "default"`, `normal.kind = "default"`, and
`sample.kind = "default"`.

## Seeds

A target's random number generator seed is a deterministic function of
its name and the global pipeline seed from `tar_option_get("seed")`.
Consequently,

    1. Each target runs with a reproducible seed so that
       different runs of the same pipeline in the same computing
       environment produce identical results.
    2. No two targets in the same pipeline share the same seed.
       Even dynamic branches have different names and thus different seeds.

You can retrieve the seed of a completed target with
`tar_meta(your_target, seed)` and run `tar_seed_set()` on the result to
locally recreate the target's initial RNG state.
[`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
does this automatically as part of recovering a workspace.

## RNG overlap

In theory, there is a risk that the pseudo-random number generator
streams of different targets will overlap and produce statistically
correlated results. (For a discussion of the motivating problem, see the
Section 6: "Random-number generation" in the `parallel` package
vignette:
[`vignette(topic = "parallel", package = "parallel")`](https://cran.rstudio.com/web/packages/parallel/vignettes/parallel.pdf).)
However, this risk is extremely small in practice, as shown by L'Ecuyer
et al. (2017)
[doi:10.1016/j.matcom.2016.05.005](https://doi.org/10.1016/j.matcom.2016.05.005)
under "A single RNG with a 'random' seed for each stream" (Section 4:
under "How to produce parallel streams and substreams"). `targets` and
`tarchetypes` take the approach discussed in the aforementioned section
of the paper using the `secretbase` package by Charlie Gao (2024)
[doi:10.5281/zenodo.10553140](https://doi.org/10.5281/zenodo.10553140) .
To generate the 32-bit integer `seed` argument of
[`set.seed()`](https://rdrr.io/r/base/Random.html) for each target,
`secretbase` generates a cryptographic hash using the SHAKE256
extendable output function (XOF). `secretbase` uses algorithms from the
`Mbed TLS` C library.

## References

- Gao C (2024). `secretbase`: Cryptographic Hash and Extendable-Output
  Functions. R package version 0.1.0,
  [doi:10.5281/zenodo.10553140](https://doi.org/10.5281/zenodo.10553140)
  .

- Pierre L'Ecuyer, David Munger, Boris Oreshkin, and Richard Simard
  (2017). Random numbers for parallel computers: Requirements and
  methods, with emphasis on GPUs. Mathematics and Computers in
  Simulation, 135, 3-17.
  [doi:10.1016/j.matcom.2016.05.005](https://doi.org/10.1016/j.matcom.2016.05.005)
  .

## See also

Other pseudo-random number generation:
[`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md),
[`tar_seed_get()`](https://docs.ropensci.org/targets/reference/tar_seed_get.md)

## Examples

``` r
seed <- tar_seed_create("target_name")
seed
#> [1] -1200009501
sample(10)
#>  [1]  7  9 10  5  8  1  6  2  4  3
tar_seed_set(seed)
sample(10)
#>  [1]  4  7  5  1  9  2 10  3  6  8
tar_seed_set(seed)
sample(10)
#>  [1]  4  7  5  1  9  2 10  3  6  8
```
