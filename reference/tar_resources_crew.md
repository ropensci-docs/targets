# Target resources: `crew` high-performance computing

Create the `crew` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional target settings.

## Usage

``` r
tar_resources_crew(
  controller = targets::tar_option_get("resources")$crew$controller,
  scale = NULL,
  seconds_timeout = targets::tar_option_get("resources")$crew$seconds_timeout
)
```

## Arguments

- controller:

  Character of length 1. If `tar_option_get("controller")` is a `crew`
  controller group, the `controller` argument of `tar_resources_crew()`
  indicates which controller in the controller group to use. If you need
  heterogeneous workers, you can leverage this argument to send
  different targets to different worker groups.

- scale:

  Deprecated in `targets` version 1.3.0.9002 (2023-10-02). No longer
  necessary.

- seconds_timeout:

  Positive numeric of length 1, optional task timeout passed to the
  `.timeout` argument of
  [`mirai::mirai()`](https://mirai.r-lib.org/reference/mirai.html)
  (after converting to milliseconds).

## Value

Object of class `"tar_resources_crew"`, to be supplied to the `crew`
argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md).

## Details

`tar_resources_crew()` accepts target-specific settings for integration
with the `crew` R package. These settings are arguments to the `push()`
method of the controller or controller group object which control things
like auto-scaling behavior and the controller to use in the case of a
controller group.

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
[`tar_resources_custom_format()`](https://docs.ropensci.org/targets/reference/tar_resources_custom_format.md),
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
  name,
  command(),
  resources = tar_resources(
    crew = tar_resources_crew(seconds_timeout = 5)
  )
)
#> <tar_stem> 
#>   name: name 
#>   description:  
#>   command:
#>     command() 
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
#>     crew: <environment> 
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
