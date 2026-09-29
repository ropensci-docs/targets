# Target resources

Create a `resources` argument for
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
or
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).

## Usage

``` r
tar_resources(
  aws = tar_option_get("resources")$aws,
  clustermq = tar_option_get("resources")$clustermq,
  crew = tar_option_get("resources")$crew,
  custom_format = tar_option_get("resources")$custom_format,
  feather = tar_option_get("resources")$feather,
  fst = tar_option_get("resources")$fst,
  future = tar_option_get("resources")$future,
  gcp = tar_option_get("resources")$gcp,
  network = tar_option_get("resources")$network,
  parquet = tar_option_get("resources")$parquet,
  qs = tar_option_get("resources")$qs,
  repository_cas = tar_option_get("resources")$repository_cas,
  url = tar_option_get("resources")$url
)
```

## Arguments

- aws:

  Output of function
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md).
  Amazon Web Services (AWS) S3 storage settings for
  `tar_target(..., repository = "aws")`. See the cloud storage section
  of <https://books.ropensci.org/targets/data.html> for details for
  instructions.

- clustermq:

  Output of function
  [`tar_resources_clustermq()`](https://docs.ropensci.org/targets/reference/tar_resources_clustermq.md).
  Optional `clustermq` settings for
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  including the `log_worker` and `template` arguments of
  [`clustermq::workers()`](https://mschubert.github.io/clustermq/reference/workers.html).
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

- crew:

  Output of function
  [`tar_resources_crew()`](https://docs.ropensci.org/targets/reference/tar_resources_crew.md)
  with target-specific settings for integration with the `crew` R
  package. These settings are arguments to the `push()` method of the
  controller or controller group object which control things like
  auto-scaling behavior and the controller to use in the case of a
  controller group.

- custom_format:

  Output of function
  [`tar_resources_custom_format()`](https://docs.ropensci.org/targets/reference/tar_resources_custom_format.md)
  with configuration details for
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  storage formats.

- feather:

  Output of function
  [`tar_resources_feather()`](https://docs.ropensci.org/targets/reference/tar_resources_feather.md).
  Non-default arguments to `arrow::read_feather()` and
  `arrow::write_feather()` for `arrow`/feather-based storage formats.
  Applies to all formats ending with the `"_feather"` suffix. For
  details on formats, see the `format` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

- fst:

  Output of function
  [`tar_resources_fst()`](https://docs.ropensci.org/targets/reference/tar_resources_fst.md).
  Non-default arguments to
  [`fst::read_fst()`](http://www.fstpackage.org/reference/write_fst.md)
  and
  [`fst::write_fst()`](http://www.fstpackage.org/reference/write_fst.md)
  for `fst`-based storage formats. Applies to all formats ending with
  `"fst"` in the name. For details on formats, see the `format` argument
  of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

- future:

  Output of function
  [`tar_resources_future()`](https://docs.ropensci.org/targets/reference/tar_resources_future.md).
  Optional `future` settings for
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
  including the `resources` argument of
  [`future::future()`](https://future.futureverse.org/reference/future.html),
  which can include values to insert in template placeholders in
  `future.batchtools` template files. This is how to supply the
  `resources` argument of
  [`future::future()`](https://future.futureverse.org/reference/future.html)
  for `targets`. Resources supplied through
  [`future::plan()`](https://future.futureverse.org/reference/plan.html)
  and
  [`future::tweak()`](https://future.futureverse.org/reference/plan.html)
  are completely ignored.

- gcp:

  Output of function
  [`tar_resources_gcp()`](https://docs.ropensci.org/targets/reference/tar_resources_gcp.md).
  Google Cloud Storage bucket settings for
  `tar_target(..., repository = "gcp")`. See the cloud storage section
  of <https://books.ropensci.org/targets/data.html> for details for
  instructions.

- network:

  Output of function
  [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md).
  Settings to configure how to handle unreliable network connections in
  the case of uploading, downloading, and checking data in situations
  that rely on network file systems or HTTP/HTTPS requests. Examples
  include retries and timeouts for internal storage management
  operations for `storage = "worker"` or `format = "file"` (on network
  file systems), `format = "url"`, `repository = "aws"`, and
  `repository = "gcp"`. These settings do not apply to actions you take
  in the custom R command of the target.

- parquet:

  Output of function
  [`tar_resources_parquet()`](https://docs.ropensci.org/targets/reference/tar_resources_parquet.md).
  Non-default arguments to `arrow::read_parquet()` and
  `arrow::write_parquet()` for `arrow`/parquet-based storage formats.
  Applies to all formats ending with the `"_parquet"` suffix. For
  details on formats, see the `format` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

- qs:

  Output of function
  [`tar_resources_qs()`](https://docs.ropensci.org/targets/reference/tar_resources_qs.md).
  Non-default arguments to
  [`qs2::qs_read()`](https://rdrr.io/pkg/qs2/man/qs_read.html) and
  [`qs2::qs_save()`](https://rdrr.io/pkg/qs2/man/qs_save.html) for
  targets with `format = "qs"`. For details on formats, see the `format`
  argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

- repository_cas:

  Output of function
  [`tar_resources_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_resources_repository_cas.md)
  with configuration details for
  [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
  storage repositories.

- url:

  Output of function
  [`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md).
  Non-default settings for storage formats ending with the `"_url"`
  suffix. These settings include the `curl` handle for extra control
  over HTTP requests. For details on formats, see the `format` argument
  of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## Value

A list of objects of class `"tar_resources"` with non-default settings
of various optional backends for data storage and high-performance
computing.

## Resources

Functions
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
and
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
each takes an optional `resources` argument to supply non-default
settings of various optional backends for data storage and
high-performance computing. The `tar_resources()` function is a helper
to supply those settings in the correct manner.

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
[`tar_resources_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_resources_repository_cas.md),
[`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md)

## Examples

``` r
# Somewhere in you target script file (usually _targets.R):
tar_target(
  name,
  command(),
  format = "qs",
  resources = tar_resources(
    qs = tar_resources_qs(preset = "fast"),
    future = tar_resources_future(resources = list(n_cores = 1))
  )
)
#> <tar_stem> 
#>   name: name 
#>   description:  
#>   command:
#>     command() 
#>   format: qs 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     future: <environment>
#>     qs: <environment> 
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
