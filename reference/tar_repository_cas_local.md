# Local content-addressable storage (CAS) repository (an experimental feature).

Local content-addressable storage (CAS) repository.

## Usage

``` r
tar_repository_cas_local(path = NULL, consistent = FALSE)
```

## Arguments

- path:

  Character string, file path to the CAS repository where all the data
  object files will be stored. `NULL` to default to
  `file.path(tar_config_get("store"), "cas")` (usually
  `"_targets/cas/"`).

- consistent:

  Logical. Set to `TRUE` if the storage platform is strongly
  read-after-write consistent. Set to `FALSE` if the platform is not
  necessarily strongly read-after-write consistent.

  A data storage system is said to have strong read-after-write
  consistency if a new object is fully available for reading as soon as
  the write operation finishes. Many modern cloud services like Amazon
  S3 and Google Cloud Storage have strong read-after-write consistency,
  meaning that if you upload an object with a PUT request, then a GET
  request immediately afterwards will retrieve the precise version of
  the object you just uploaded.

  Some storage systems do not have strong read-after-write consistency.
  One example is network file systems (NFS). On a computing cluster, if
  one node creates a file on an NFS, then there is a delay before other
  nodes can access the new file. `targets` handles this situation by
  waiting for the new file to appear with the correct hash before
  attempting to use it in downstream computations. `consistent = FALSE`
  imposes a waiting period in which `targets` repeatedly calls the
  `exists` method until the file becomes available (at time intervals
  configurable with
  [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md)).
  These extra calls to `exists` may come with a little extra latency /
  computational burden, but on systems which are not strongly
  read-after-write consistent, it is the only way `targets` can safely
  use the current results for downstream computations.

## Value

A character string from
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
which may be passed to the `repository` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
or
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
to use a local CAS system.

## Details

Pass to the `repository` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
or
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
to use a local CAS system.

## Content-addressable storage

Normally, `targets` organizes output data based on target names. For
example, if a pipeline has a single target `x` with default settings,
then
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
saves the output data to the file `_targets/objects/x`. When the output
of `x` changes,
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
overwrites `_targets/objects/x`. In other words, no matter how many
changes happen to `x`, the data store always looks like this:

    _targets/
        meta/
            meta
        objects/
            x

By contrast, with content-addressable storage (CAS), `targets` organizes
outputs based on the hashes of their contents. The name of each output
file is its hash, and the metadata maps these hashes to target names.
For example, suppose target `x` has
`repository = tar_repository_cas_local("my_cas")`. When the output of
`x` changes,
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
creates a new file inside `my_cas/` without overwriting or deleting any
other files in that folder. If you run
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
three different times with three different values of `x`, then storage
will look like this:

    _targets/
        meta/
            meta
    my_cas/
        1fffeb09ad36e84a
        68328d833e6361d3
        798af464fb2f6b30

The next call to `tar_read(x)` uses `tar_meta(x)$data` to look up the
current hash of `x`. If `tar_meta(x)$data` returns `"1fffeb09ad36e84a"`,
then `tar_read(x)` returns the data from `my_cas/1fffeb09ad36e84a`.
Files `my_cas/68328d833e6361d3` and `my_cas/798af464fb2f6b30` are left
over from previous values of `x`.

Because CAS accumulates historical data objects, it is ideal for data
versioning and collaboration. If you commit the `_targets/meta/meta`
file to version control alongside the source code, then you can revert
to a previous state of your pipeline with all your targets up to date,
and a colleague can leverage your hard-won results using a fork of your
code and metadata.

The downside of CAS is the cost of accumulating many data objects over
time. Most pipelines that use CAS should have a garbage collection
system or retention policy to remove data objects when they are no
longer needed.

The
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
function lets you create your own CAS system for `targets`. You can
supply arbitrary custom methods to upload, download, and check for the
existence of data objects. Your custom CAS system can exist locally on a
shared file system or remotely on the cloud (e.g. in an AWS S3 bucket).
See the "Repository functions" section and the documentation of
individual arguments for advice on how to write your own methods.

The `tar_repository_cas_local()` function has an example CAS system
based on a local folder on disk. It uses
[`tar_cas_u()`](https://docs.ropensci.org/targets/reference/tar_cas_u.md)
for uploads,
[`tar_cas_d()`](https://docs.ropensci.org/targets/reference/tar_cas_d.md)
for downloads, and
[`tar_cas_l()`](https://docs.ropensci.org/targets/reference/tar_cas_l.md)
for listing keys.

## See also

Other content-addressable storage:
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md),
[`tar_repository_cas_local_gc()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local_gc.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  repository <- tar_repository_cas_local("cas")
  write_file <- function(object) {
    writeLines(as.character(object), "file.txt")
    "file.txt"
  }
  list(
    tar_target(x, c(2L, 4L), repository = repository),
    tar_target(
      y,
      x,
      pattern = map(x),
      format = "qs",
      repository = repository
    ),
    tar_target(z, write_file(y), format = "file", repository = repository)
  )
})
tar_make()
tar_read(y)
tar_read(z)
list.files("cas")
tar_meta(any_of(c("x", "z")), fields = any_of("data"))
})
}
```
