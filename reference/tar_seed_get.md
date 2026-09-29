# Get the random number generator seed of the target currently running.

Get the random number generator seed of the target currently running.

## Usage

``` r
tar_seed_get(default = 1L)
```

## Arguments

- default:

  Integer, value to return if `tar_seed_get()` is called on its own
  outside a `targets` pipeline. Having a default lets users run things
  without
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  which helps peel back layers of code and troubleshoot bugs.

## Value

Integer of length 1. If invoked inside a `targets` pipeline, the return
value is the seed of the target currently running, which is a
deterministic function of the target name. Otherwise, the return value
is `default`.

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
`tar_meta(your_target, seed)` and run
[`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md)
on the result to locally recreate the target's initial RNG state.
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
[`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md)

## Examples

``` r
tar_seed_get()
#> [1] 1
tar_seed_get(default = 123L)
#> [1] 123
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script(tar_target(returns_seed, tar_seed_get()), ask = FALSE)
tar_make()
tar_read(returns_seed)
})
}
```
