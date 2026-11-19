# Target resources: Google Cloud Platform (GCP) Google Cloud Storage (GCS)

Create the `gcp` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional settings for Google Cloud Storage for targets with
`tar_target(..., repository = "gcp")`. See the `format` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
for details.

## Usage

``` r
tar_resources_gcp(
  bucket = targets::tar_option_get("resources")$gcp$bucket,
  prefix = targets::tar_option_get("resources")$gcp$prefix,
  predefined_acl = targets::tar_option_get("resources")$gcp$predefined_acl,
  max_tries = targets::tar_option_get("resources")$gcp$max_tries,
  verbose = targets::tar_option_get("resources")$gcp$verbose
)
```

## Arguments

- bucket:

  Character of length 1, name of an existing bucket to upload and
  download the return values of the affected targets during the
  pipeline.

- prefix:

  Character of length 1, "directory path" in the bucket where your
  target definition object and metadata will go. Please supply an
  explicit prefix unique to your `targets` project. In the future,
  `targets` will begin requiring explicitly user-supplied prefixes.
  (This last note was added on 2023-08-24: `targets` version
  1.2.2.9000.)

- predefined_acl:

  Character of length 1, user access to the object. See
  [`?googleCloudStorageR::gcs_upload`](https://cloudyr.github.io/googleCloudStorageR//reference/gcs_upload.html)
  for possible values. Defaults to `"private"`.

- max_tries:

  Positive integer of length 1, number of tries accessing a network
  resource on GCP.

- verbose:

  Logical of length 1, whether to print extra messages like progress
  bars during uploads and downloads. Defaults to `FALSE`.

## Value

Object of class `"tar_resources_gcp"`, to be supplied to the `gcp`
argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md).

## Details

See the cloud storage section of
<https://books.ropensci.org/targets/data.html> for details for
instructions.

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
  repository = "gcp",
  resources = tar_resources(
    gcp = tar_resources_gcp(
      bucket = "yourbucketname",
      prefix = "_targets"
    ),
    qs = tar_resources_qs(preset = "fast"),
  )
)
#> <tar_stem> 
#>   name: name 
#>   description:  
#>   command:
#>     command() 
#>   format: qs 
#>   repository: gcp 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     gcp: <environment>
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
