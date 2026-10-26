# Target resources for custom storage formats

Create the `repository_cas` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional target settings for custom storage formats.

## Usage

``` r
tar_resources_repository_cas(
  envvars = targets::tar_option_get("resources")$repository_cas$envvars
)
```

## Arguments

- envvars:

  Named character vector of environment variables. These environment
  variables are temporarily set just before each call to the storage
  methods you define in
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md).
  Specific methods like `read` can retrieve values from these
  environment variables using
  [`Sys.getenv()`](https://rdrr.io/r/base/Sys.getenv.html). Set
  `envvars` to `NULL` to omit entirely.

## Value

Object of class `"tar_resources_repository_cas"`, to be supplied to the
`repository_cas` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md).

## Details

`tar_resources_repository_cas()` accepts target-specific settings to
customize
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
storage repositories.

## Resources

Functions
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
and
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
each takes an optional `resources` argument to supply non-default
settings of various optional backends for data storage and
high-performance computing. The
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
function is a helper to supply those settings in the correct manner.

In `targets` version 0.12.2 and above, resources are inherited
one-by-one in nested fashion from `tar_option_get("resources")`. For
example, suppose you set
`tar_option_set(resources = tar_resources(aws = my_aws))`, where
`my_aws` equals `tar_resources_aws(bucket = "x", prefix = "y")`. Then,
`tar_target(data, get_data()` will have bucket `"x"` and prefix `"y"`.
In addition, if `new_resources` equals
`tar_resources(aws = tar_resources_aws(bucket = "z")))`, then
`tar_target(data, get_data(), resources = new_resources)` will use the
new bucket `"z"`, but it will still use the prefix `"y"` supplied
through
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).
(In `targets` 0.12.1 and below, options like `prefix` do not carry over
from
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
if you supply non-default resources to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).)

## See also

Other resources:
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md),
[`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md),
[`tar_resources_clustermq()`](https://docs.ropensci.org/targets/reference/tar_resources_clustermq.md),
[`tar_resources_crew()`](https://docs.ropensci.org/targets/reference/tar_resources_crew.md),
[`tar_resources_custom_format()`](https://docs.ropensci.org/targets/reference/tar_resources_custom_format.md),
[`tar_resources_feather()`](https://docs.ropensci.org/targets/reference/tar_resources_feather.md),
[`tar_resources_fst()`](https://docs.ropensci.org/targets/reference/tar_resources_fst.md),
[`tar_resources_future()`](https://docs.ropensci.org/targets/reference/tar_resources_future.md),
[`tar_resources_gcp()`](https://docs.ropensci.org/targets/reference/tar_resources_gcp.md),
[`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md),
[`tar_resources_parquet()`](https://docs.ropensci.org/targets/reference/tar_resources_parquet.md),
[`tar_resources_qs()`](https://docs.ropensci.org/targets/reference/tar_resources_qs.md),
[`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md)

## Examples

``` r
# Somewhere in you target script file (usually _targets.R):
tar_target(
  name = target_name,
  command = data.frame(x = 1),
  repository = tar_repository_cas(
    upload = function(key, path) {
      if (dir.exists(path)) {
        stop("This CAS repository does not support directory outputs.")
      }
      if (!file.exists("cas")) {
        dir.create("cas", recursive = TRUE)
      }
      file.copy(path, file.path("cas", key))
    },
    download = function(key, path) {
      file.copy(file.path("cas", key), path)
    },
    exists = function(key) {
      file.exists(file.path("cas", key))
    }
  ),
  resources = tar_resources(
    repository_cas = tar_resources_repository_cas(
      envvars = c(AUTHENTICATION_CREDENTIALS = "...")
    )
  )
)
#> <tar_stem> 
#>   name: target_name 
#>   description:  
#>   command:
#>     data.frame(x = 1) 
#>   format: rds 
#>   repository: repository_cas&upload=ewogICAgaWYgKGRpci5leGlzdHMocGF0aCkpIHsKICAgICAgICBzdG9wKCJUaGlzIENBUyByZXBvc2l0b3J5IGRvZXMgbm90IHN1cHBvcnQgZGlyZWN0b3J5IG91dHB1dHMuIikKICAgIH0KICAgIGlmICghZmlsZS5leGlzdHMoImNhcyIpKSB7CiAgICAgICAgZGlyLmNyZWF0ZSgiY2FzIiwgcmVjdXJzaXZlID0gVFJVRSkKICAgIH0KICAgIGZpbGUuY29weShwYXRoLCBmaWxlLnBhdGgoImNhcyIsIGtleSkpCn0&download=ewogICAgZmlsZS5jb3B5KGZpbGUucGF0aCgiY2FzIiwga2V5KSwgcGF0aCkKfQ&exists=ewogICAgZmlsZS5leGlzdHMoZmlsZS5wYXRoKCJjYXMiLCBrZXkpKQp9&list=&consistent=RkFMU0U 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     repository_cas: <environment> 
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
```
