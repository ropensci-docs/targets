# Local CAS garbage collection

Garbage collection for a local content-addressable storage system.

## Usage

``` r
tar_repository_cas_local_gc(
  path = NULL,
  store = targets::tar_config_get("store")
)
```

## Arguments

- path:

  Character string, file path to the CAS repository where all the data
  object files will be stored. `NULL` to default to
  `file.path(tar_config_get("store"), "cas")` (usually
  `"_targets/cas/"`).

- store:

  Character of length 1, path to the `targets` data store. Defaults to
  `tar_config_get("store")`, which in turn defaults to `_targets/`. When
  you set this argument, the value of `tar_config_get("store")` is
  temporarily changed for the current function call. See
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  for details about how to set the data store path persistently for a
  project.

## Value

`NULL` (invisibly). Called for its side effects. Removes files from the
CAS repository at `path`.

## Details

Deletes all the files in the local CAS which are not in
`tar_meta(targets_only = TRUE)$data`, including all locally saved
historical data of the pipeline. This clears disk space, but at the
expense of removing historical data and data from other colleagues who
worked on the same project.

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

The
[`tar_repository_cas_local()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local.md)
function has an example CAS system based on a local folder on disk. It
uses
[`tar_cas_u()`](https://docs.ropensci.org/targets/reference/tar_cas_u.md)
for uploads,
[`tar_cas_d()`](https://docs.ropensci.org/targets/reference/tar_cas_d.md)
for downloads, and
[`tar_cas_l()`](https://docs.ropensci.org/targets/reference/tar_cas_l.md)
for listing keys.

## See also

Other content-addressable storage:
[`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md),
[`tar_repository_cas_local()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(seed = NA, repository = tar_repository_cas_local())
  list(tar_target(x, sample.int(n = 9e9, size = 1)))
})
for (index in seq_len(3)) tar_make(reporter = "silent")
list.files("_targets/cas")
tar_repository_cas_local_gc()
list.files("_targets/cas")
tar_meta(names = any_of("x"), fields = any_of("data"))
})
}
```
