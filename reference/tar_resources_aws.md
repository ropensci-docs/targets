# Target resources: Amazon Web Services (AWS) S3 storage

Create the `aws` argument of
[`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
to specify optional settings to AWS for
`tar_target(..., repository = "aws")`. See the `format` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
for details.

## Usage

``` r
tar_resources_aws(
  bucket = targets::tar_option_get("resources")$aws$bucket,
  prefix = targets::tar_option_get("resources")$aws$prefix,
  region = targets::tar_option_get("resources")$aws$region,
  endpoint = targets::tar_option_get("resources")$aws$endpoint,
  s3_force_path_style = targets::tar_option_get("resources")$aws$s3_force_path_style,
  part_size = targets::tar_option_get("resources")$aws$part_size,
  page_size = targets::tar_option_get("resources")$aws$page_size,
  max_tries = targets::tar_option_get("resources")$aws$max_tries,
  seconds_timeout = targets::tar_option_get("resources")$aws$seconds_timeout,
  close_connection = targets::tar_option_get("resources")$aws$close_connection,
  verbose = targets::tar_option_get("resources")$aws$verbose,
  ...
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

- region:

  Character of length 1, AWS region containing the S3 bucket. Set to
  `NULL` to use the default region.

- endpoint:

  Character of length 1, URL endpoint for S3 storage. Defaults to the
  Amazon AWS endpoint if `NULL`. Example: To use the S3 protocol with
  Google Cloud Storage, set
  `endpoint = "https://storage.googleapis.com"` and `region = "auto"`.
  (A custom endpoint may require that you explicitly set a custom region
  directly in `tar_resources_aws()`. `region = "auto"` happens to work
  with Google Cloud.) Also make sure to create HMAC access keys in the
  Google Cloud Storage console (under Settings =\> Interoperability) and
  set the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment
  variables accordingly. After that, you should be able to use S3
  storage formats with Google Cloud storage buckets. There is one
  limitation, however: even if your bucket has object versioning turned
  on, `targets` may fail to record object versions. Google Cloud Storage
  in particular has this incompatibility.

- s3_force_path_style:

  Logical of length 1, whether to use path-style addressing for S3
  requests.

- part_size:

  Positive numeric of length 1, number of bytes for each part of a
  multipart upload. (Except the last part, which is the remainder.) In a
  multipart upload, each part must be at least 5 MB. The default value
  of the `part_size` argument is `5 * (2 ^ 20)`.

- page_size:

  Positive integer of length 1, number of items in each page for
  paginated HTTP requests such as listing objects.

- max_tries:

  Positive integer of length 1, maximum number of attempts to access a
  network resource on AWS.

- seconds_timeout:

  Positive numeric of length 1, number of seconds until an HTTP
  connection times out.

- close_connection:

  Logical of length 1, whether to close HTTP connections immediately.

- verbose:

  Logical of length 1, whether to print console messages when running
  computationally expensive operations such as listing objects in a
  large bucket.

- ...:

  Named arguments to functions in
  [`paws.storage::s3()`](https://paws-r.r-universe.dev/paws.storage/reference/s3.html)
  to manage S3 storage. The documentation of these specific functions is
  linked from `https://www.paws-r-sdk.com/docs/s3/`. The configurable
  functions themselves are:

  - `paws.storage::s3()$head_object()`

  - `paws.storage::s3()$get_object()`

  - `paws.storage::s3()$delete_object()`

  - `paws.storage::s3()$put_object()`

  - `paws.storage::s3()$create_multipart_upload()`

  - `paws.storage::s3()$abort_multipart_upload()`

  - `paws.storage::s3()$complete_multipart_upload()`

  - `paws.storage::s3()$upload_part()` The named arguments in `...` must
    not be any of `"bucket"`, `"Bucket"`, `"key"`, `"Key"`, `"prefix"`,
    `"region"`, `"part_size"`, `"endpoint"`, `"version"`, `"VersionId"`,
    `"body"`, `"Body"`, `"metadata"`, `"Metadata"`, `"UploadId"`,
    `"MultipartUpload"`, or `"PartNumber"`.

## Value

Object of class `"tar_resources_aws"`, to be supplied to the `aws`
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
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_target(
  name,
  command(),
  format = "qs",
  repository = "aws",
  resources = tar_resources(
    aws = tar_resources_aws(
      bucket = "yourbucketname",
      prefix = "_targets"
    ),
    qs = tar_resources_qs(preset = "fast"),
  )
)
}
```
