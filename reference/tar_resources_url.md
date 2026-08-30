# Target resources: URL storage formats

Create the `url` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional settings for URL storage formats. See the `format`
argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
for details.

## Usage

``` r
tar_resources_url(
  handle = targets::tar_option_get("resources")$url$handle,
  max_tries = targets::tar_option_get("resources")$url$max_tries,
  seconds_interval = targets::tar_option_get("resources")$url$seconds_interval,
  seconds_timeout = targets::tar_option_get("resources")$url$seconds_interval
)
```

## Arguments

- handle:

  Object returned by
  [`curl::new_handle`](https://jeroen.r-universe.dev/curl/reference/handle.html)
  or `NULL`. Defaults to `NULL`.

- max_tries:

  Positive integer of length 1, maximum number of tries to access a URL.

- seconds_interval:

  Nonnegative numeric of length 1, number of seconds to wait between
  individual retries while attempting to connect to the URL. Use
  [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md)
  instead.

- seconds_timeout:

  Nonnegative numeric of length 1, number of seconds to wait before
  timing out while trying to connect to the URL. Use
  [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md)
  instead.

## Value

Object of class `"tar_resources_url"`, to be supplied to the url
argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md).

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
[`tar_resources_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_resources_repository_cas.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
# Somewhere in you target script file (usually _targets.R):
tar_target(
  name,
  command(),
  format = "url",
  resources = tar_resources(
    url = tar_resources_url(handle = curl::new_handle())
  )
)
}
```
