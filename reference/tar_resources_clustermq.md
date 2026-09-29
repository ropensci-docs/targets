# Target resources: `clustermq` high-performance computing

Create the `clustermq` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional high-performance computing settings for
[`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md).
For details, see the documentation of the `clustermq` R package and the
corresponding argument names in this help file.

## Usage

``` r
tar_resources_clustermq(
  template = targets::tar_option_get("resources")$clustermq$template
)
```

## Arguments

- template:

  Named list, `template` argument to
  [`clustermq::workers()`](https://mschubert.github.io/clustermq/reference/workers.html).
  Defaults to an empty list.

## Value

Object of class `"tar_resources_clustermq"`, to be supplied to the
`clustermq` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md).

## Details

`clustermq` workers are *persistent*, so there is not a one-to-one
correspondence between workers and targets. The `clustermq` resources
apply to the workers, not the targets. So the correct way to assign
`clustermq` resources is through
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md),
not
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).
`clustermq` resources in individual
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
calls will be ignored.

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
[`tar_resources_crew()`](https://docs.ropensci.org/targets/reference/tar_resources_crew.md),
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
    clustermq = tar_resources_clustermq(template = list(n_cores = 2))
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
#>     clustermq: <environment> 
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
