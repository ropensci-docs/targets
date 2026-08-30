# Target resources for custom storage formats

Create the `custom_format` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional target settings for custom storage formats.

## Usage

``` r
tar_resources_custom_format(
  envvars = targets::tar_option_get("resources")$custom_format$envvars
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

Object of class `"tar_resources_custom_format"`, to be supplied to the
`custom_format` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md).

## Details

`tar_resources_custom_format()` accepts target-specific settings to
customize
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
storage formats.

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
[`tar_resources_feather()`](https://docs.ropensci.org/targets/reference/tar_resources_feather.md),
[`tar_resources_fst()`](https://docs.ropensci.org/targets/reference/tar_resources_fst.md),
[`tar_resources_future()`](https://docs.ropensci.org/targets/reference/tar_resources_future.md),
[`tar_resources_gcp()`](https://docs.ropensci.org/targets/reference/tar_resources_gcp.md),
[`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md),
[`tar_resources_parquet()`](https://docs.ropensci.org/targets/reference/tar_resources_parquet.md),
[`tar_resources_qs()`](https://docs.ropensci.org/targets/reference/tar_resources_qs.md),
[`tar_resources_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_resources_repository_cas.md),
[`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md)

## Examples

``` r
# Somewhere in you target script file (usually _targets.R):
tar_target(
  name = target_name,
  command = data.frame(x = 1),
  format = tar_format(
    read = function(path) {
      readRDS(file = path)
    },
    write = function(object, path) {
      version <- as.integer(Sys.getenv("SERIALIZATION", unset = "2"))
      saveRDS(object = object, file = path, version = version)
    }
  ),
  resources = tar_resources(
    custom_format = tar_resources_custom_format(
      envvars = c(SERIALIZATION = "3")
    )
  )
)
#> <tar_stem> 
#>   name: target_name 
#>   description:  
#>   command:
#>     data.frame(x = 1) 
#>   format: format_custom&read=ewogICAgcmVhZFJEUyhmaWxlID0gcGF0aCkKfQ&write=ewogICAgdmVyc2lvbiA8LSBhcy5pbnRlZ2VyKFN5cy5nZXRlbnYoIlNFUklBTElaQVRJT04iLCB1bnNldCA9ICIyIikpCiAgICBzYXZlUkRTKG9iamVjdCA9IG9iamVjdCwgZmlsZSA9IHBhdGgsIHZlcnNpb24gPSB2ZXJzaW9uKQp9&marshal=&unmarshal=&convert=&copy=&repository= 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     custom_format: <environment> 
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
