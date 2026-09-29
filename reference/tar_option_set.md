# Set target options.

Set target options, including default arguments to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
such as packages, storage format, iteration type, and cue. Only the
non-null arguments are actually set as options. See currently set
options with
[`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md).
To use `tar_option_set()` effectively, put it in your workflow's target
script file (default: `_targets.R`) before calls to
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
or
[`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md).

## Usage

``` r
tar_option_set(
  tidy_eval = NULL,
  packages = NULL,
  imports = NULL,
  library = NULL,
  envir = NULL,
  format = NULL,
  repository = NULL,
  repository_meta = NULL,
  iteration = NULL,
  error = NULL,
  memory = NULL,
  garbage_collection = NULL,
  deployment = NULL,
  priority = NULL,
  backoff = NULL,
  resources = NULL,
  storage = NULL,
  retrieval = NULL,
  cue = NULL,
  description = NULL,
  debug = NULL,
  workspaces = NULL,
  workspace_on_error = NULL,
  seed = NULL,
  controller = NULL,
  trust_timestamps = NULL,
  trust_object_timestamps = NULL
)
```

## Arguments

- tidy_eval:

  Logical, whether to enable tidy evaluation when interpreting `command`
  and `pattern`. If `TRUE`, you can use the "bang-bang" operator `!!` to
  programmatically insert the values of global objects.

- packages:

  Character vector of packages to load right before the target runs or
  the output data is reloaded for downstream targets. Use
  `tar_option_set()` to set packages globally for all subsequent targets
  you define.

- imports:

  Character vector of package names. For every package listed, `targets`
  tracks every dataset and every object in the package namespace as if
  it were part of the global namespace. As an example, say you have a
  package called `customAnalysisPackage` which contains an object called
  `analysis_function()`. If you write
  `tar_option_set(imports = "yourAnalysisPackage")` in your target
  script file (default: `_targets.R`), then a function called
  `"analysis_function"` will show up in the
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  graph, and any targets or functions referring to the symbol
  `"analysis_function"` will depend on the function
  `analysis_function()` from package `yourAnalysisPackage`. This is best
  combined with `tar_option_set(packages = "yourAnalysisPackage")` so
  that `analysis_function()` can actually be called in your code.

  There are several important limitations: 1. Namespaced calls, e.g.
  `yourAnalysisPackage::analysis_function()`, are ignored because of the
  limitations in
  [`codetools::findGlobals()`](https://rdrr.io/pkg/codetools/man/findGlobals.html)
  which powers the static code analysis capabilities of `targets`. 2.
  The `imports` option only looks at R objects and R code. It not
  account for low-level compiled code such as C/C++ or Fortran. 3. If
  you supply multiple packages, e.g.
  `tar_option_set(imports = c("p1", "p2"))`, then the objects in `p1`
  override the objects in `p2` if there are name conflicts. 4.
  Similarly, objects in `tar_option_get("envir")` override everything in
  `tar_option_get("imports")`.

- library:

  Character vector of library paths to try when loading `packages`.

- envir:

  Environment containing functions and global objects common to all
  targets in the pipeline. The `envir` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and related functions always overrides the current value of
  `tar_option_get("envir")` in the current R session just before running
  the target script file, so whenever you need to set an alternative
  `envir`, you should always set it with `tar_option_set()` from within
  the target script file. In other words, if you call
  `tar_option_set(envir = envir1)` in an interactive session and then
  `tar_make(envir = envir2, callr_function = NULL)`, then `envir2` will
  be used.

  If `envir` is the global environment, all the promise objects are
  diffused before sending the data to parallel workers in
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  and
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  but otherwise the environment is unmodified. This behavior improves
  performance by decreasing the size of data sent to workers.

  If `envir` is not the global environment, then it should at least
  inherit from the global environment or base environment so `targets`
  can access attached packages. In the case of a non-global `envir`,
  `targets` attempts to remove potentially high memory objects that come
  directly from `targets`. That includes
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  objects of class `"tar_target"`, as well as objects of class
  `"tar_pipeline"` or `"tar_algorithm"`. This behavior improves
  performance by decreasing the size of data sent to workers.

  Package environments should not be assigned to `envir`. To include
  package objects as upstream dependencies in the pipeline, assign the
  package to the `packages` and `imports` arguments of
  `tar_option_set()`.

- format:

  Optional storage format for the target's return value. With the
  exception of `format = "file"`, each target gets a file in
  `_targets/objects`, and each format is a different way to save and
  load this file. See the "Storage formats" section for a detailed list
  of possible data storage formats.

- repository:

  Character of length 1, remote repository for target storage. Choices:

  - `"local"`: file system of the local machine.

  - `"aws"`: Amazon Web Services (AWS) S3 bucket. Can be configured with
    a non-AWS S3 bucket using the `endpoint` argument of
    [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md),
    but versioning capabilities may be lost in doing so. See the cloud
    storage section of <https://books.ropensci.org/targets/data.html>
    for details for instructions.

  - `"gcp"`: Google Cloud Platform storage bucket. See the cloud storage
    section of <https://books.ropensci.org/targets/data.html> for
    details for instructions.

  - A character string from
    [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
    for content-addressable storage.

  Note: if `repository` is not `"local"` and `format` is `"file"` then
  the target should create a single output file. That output file is
  uploaded to the cloud and tracked for changes where it exists in the
  cloud. As of `targets` version 1.11.0 and higher, the local file is no
  longer deleted after the target runs.

- repository_meta:

  Character of length 1 with the same values as `repository` but
  excluding content-addressable storage (`"aws"`, `"gcp"`, `"local"`).
  Cloud repository for the metadata text files in `_targets/meta/`,
  including target metadata and progress data. Also enables cloud backup
  of workspace files in `_targets/workspaces/` which can be downloaded
  with
  [`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md).
  `repository_meta` defaults to `tar_option_get("repository")` except in
  the case of content-addressable storage (CAS). When
  `tar_option_get("repository")` is a CAS repository, the default value
  of `repository_meta` is `"local"`.

- iteration:

  Character of length 1, name of the iteration mode of the target.
  Choices:

  - `"vector"`: branching happens with
    [`vctrs::vec_slice()`](https://vctrs.r-lib.org/reference/vec_slice.html)
    and aggregation happens with
    [`vctrs::vec_c()`](https://vctrs.r-lib.org/reference/vec_c.html).

  - `"list"`, branching happens with `[[]]` and aggregation happens with
    [`list()`](https://rdrr.io/r/base/list.html).

  - `"group"`:
    [`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html)-like
    functionality to branch over subsets of a non-dynamic data frame.
    For `iteration = "group"`, the target must not by dynamic (the
    `pattern` argument of
    [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
    must be left `NULL`). The target's return value must be a data frame
    with a special `tar_group` column of consecutive integers from 1
    through the number of groups. Each integer designates a group, and a
    branch is created for each collection of rows in a group. See the
    [`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.md)
    function to see how you can create the special `tar_group` column
    with
    [`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html).

- error:

  Character of length 1, what to do if the target stops and throws an
  error. Options:

  - `"stop"`: the whole pipeline stops and throws an error.

  - `"continue"`: the whole pipeline keeps going.

  - `"null"`: The errored target continues and returns `NULL`. The data
    hash is deliberately wrong so the target is not up to date for the
    next run of the pipeline. In addition, as of `targets` version
    1.8.0.9011, a value of `NULL` is given to upstream dependencies with
    `error = "null"` if loading fails.

  - `"abridge"`: any currently running targets keep running, but no new
    targets launch after that.

  - `"trim"`: all currently running targets stay running. A queued
    target is allowed to start if:

    1.  It is not downstream of the error, and

    2.  It is not a sibling branch from the same
        [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
        call (if the error happened in a dynamic branch).

    The idea is to avoid starting any new work that the immediate error
    impacts. `error = "trim"` is just like `error = "abridge"`, but it
    allows potentially healthy regions of the dependency graph to begin
    running. (Visit <https://books.ropensci.org/targets/debugging.html>
    to learn how to debug targets using saved workspaces.)

- memory:

  Character of length 1, memory strategy. Possible values:

  - `"auto"` (default): equivalent to `memory = "transient"` in almost
    all cases. But to avoid superfluous reads from disk,
    `memory = "auto"` is equivalent to `memory = "persistent"` for for
    non-dynamically-branched targets that other targets dynamically
    branch over. For example: if your pipeline has
    `tar_target(name = y, command = x, pattern = map(x))`, then
    `tar_target(name = x, command = f(), memory = "auto")` will use
    persistent memory for `x` in order to avoid rereading all of `x` for
    every branch of `y`.

  - `"transient"`: the target gets unloaded after every new target
    completes. Either way, the target gets automatically loaded into
    memory whenever another target needs the value.

  - `"persistent"`: the target stays in memory until the end of the
    pipeline (unless `storage` is `"worker"`, in which case `targets`
    unloads the value from memory right after storing it in order to
    avoid sending copious data over a network).

  For cloud-based file targets (e.g. `format = "file"` with
  `repository = "aws"`), the `memory` option applies to the temporary
  local copy of the file: `"persistent"` means it remains until the end
  of the pipeline and is then deleted, and `"transient"` means it gets
  deleted as soon as possible. The former conserves bandwidth, and the
  latter conserves local storage.

- garbage_collection:

  A non-negative integer. If `0`, do not run garbage collection. If a
  positive integer `n`, run
  [`base::gc()`](https://rdrr.io/r/base/gc.html) just before every
  `n`'th target that runs. For the purposes of running garbage
  collection with this setting, each R process (whether the local
  process a parallel worker) maintains its own independent count of the
  number of targets that ran so far. The `garbage_collection` option in
  `tar_option_set()` is independent of the argument of the same name in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).

- deployment:

  Character of length 1. If `deployment` is `"main"`, then the target
  will run on the central controlling R process. Otherwise, if
  `deployment` is `"worker"` and you set up the pipeline with
  distributed/parallel computing, then the target runs on a parallel
  worker. For more on distributed/parallel computing in `targets`,
  please visit <https://books.ropensci.org/targets/crew.html>.

- priority:

  Deprecated on 2025-04-08 (`targets` version 1.10.1.9013). `targets`
  has moved to a more efficient scheduling algorithm
  (<https://github.com/ropensci/targets/issues/1458>) which cannot
  support priorities. The `priority` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  no longer has a reliable effect on execution order.

- backoff:

  An object from
  [`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md)
  configuring the exponential backoff algorithm of the pipeline. See
  [`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md)
  for details. A numeric argument for `backoff` is still allowed, but
  deprecated.

- resources:

  Object returned by
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  with optional settings for high-performance computing functionality,
  alternative data storage formats, and other optional capabilities of
  `targets`. See
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  for details.

- storage:

  Character string to control when the output of the target is saved to
  storage. Only relevant when using `targets` with parallel workers
  (<https://books.ropensci.org/targets/crew.html>). Must be one of the
  following values:

  - `"worker"` (default): the worker saves/uploads the value.

  - `"main"`: the target's return value is sent back to the host machine
    and saved/uploaded locally.

  - `"none"`: `targets` makes no attempt to save the result of the
    target to storage in the location where `targets` expects it to be.
    Saving to storage is the responsibility of the user. Use with
    caution.

- retrieval:

  Character string to control when the current target loads its
  dependencies into memory before running. (Here, a "dependency" is
  another target upstream that the current one depends on.) Only
  relevant when using `targets` with parallel workers
  (<https://books.ropensci.org/targets/crew.html>). Must be one of the
  following values:

  - `"auto"` (default): equivalent to `retrieval = "worker"` in almost
    all cases. But to avoid unnecessary reads from disk,
    `retrieval = "auto"` is equivalent to `retrieval = "main"` for
    dynamic branches that branch over non-dynamic targets. For example:
    if your pipeline has `tar_target(x, command = f())`, then
    `tar_target(y, command = x, pattern = map(x), retrieval = "auto")`
    will use `"main"` retrieval in order to avoid rereading all of `x`
    for every branch of `y`.

  - `"worker"`: the worker loads the target's dependencies.

  - `"main"`: the target's dependencies are loaded on the host machine
    and sent to the worker before the target runs.

  - `"none"`: `targets` makes no attempt to load its dependencies. With
    `retrieval = "none"`, loading dependencies is the responsibility of
    the user. Use with caution.

- cue:

  An optional object from
  [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
  to customize the rules that decide whether the target is up to date.

- description:

  Character of length 1, a custom free-form human-readable text
  description of the target. Descriptions appear as target labels in
  functions like
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
  and
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
  and they let you select subsets of targets for the `names` argument of
  functions like
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
  For example,
  `tar_manifest(names = tar_described_as(starts_with("survival model")))`
  lists all the targets whose descriptions start with the character
  string `"survival model"`.

- debug:

  Character vector of names of targets to run in debug mode. To use
  effectively, you must set `callr_function = NULL` and restart your R
  session just before running. You should also
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  or
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md).
  For any target mentioned in `debug`, `targets` will force the target
  to run locally (with `tar_cue(mode = "always")` and
  `deployment = "main"` in the settings) and pause in an interactive
  debugger to help you diagnose problems. This is like inserting a
  [`browser()`](https://rdrr.io/r/base/browser.html) statement at the
  beginning of the target's expression, but without invalidating any
  targets.

- workspaces:

  Character vector of target names. Could be non-branching targets,
  whole dynamic branching targets, or individual branch names.
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and friends will save workspace files for these targets even if the
  targets are skipped. Workspace files help with debugging. See
  [`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
  for details about workspaces.

- workspace_on_error:

  Logical of length 1, whether to save a workspace file for each target
  that throws an error. Workspace files help with debugging. See
  [`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
  for details about workspaces.

- seed:

  Integer of length 1, seed for generating target-specific pseudo-random
  number generator seeds. These target-specific seeds are deterministic
  and depend on `tar_option_get("seed")` and the target name.
  Target-specific seeds are safely and reproducibly applied to each
  target's command, and they are stored in the metadata and retrievable
  with
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md)
  or
  [`tar_seed()`](https://docs.ropensci.org/targets/reference/tar_seed.md).

  Either the user or third-party packages built on top of `targets` may
  still set seeds inside the command of a target. For example, some
  target factories in the `tarchetypes` package assigns
  replicate-specific seeds for the purposes of reproducible
  within-target batched replication. In cases like these, the effect of
  the target-specific seed saved in the metadata becomes irrelevant and
  the seed defined in the command applies.

  The `seed` option can also be `NA` to disable automatic seed-setting.
  Any targets defined while `tar_option_get("seed")` is `NA` will not
  set a seed. In this case, those targets will never be up to date
  unless they have `cue = tar_cue(seed = FALSE)`.

- controller:

  A controller or controller group object produced by the `crew` R
  package. `crew` brings auto-scaled distributed computing to
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).

- trust_timestamps:

  Logical of length 1, whether to use file system modification
  timestamps to check whether the target output data files in are up to
  date. This is an advanced setting and usually does not need to be set
  by the user except on old or difficult platforms.

  If `trust_timestamps` was reset with
  [`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md)
  or never set at all (recommended) then `targets` makes a decision
  based on the type of file system of the given file.

  If `trust_timestamps` is `TRUE` (default), then `targets` looks at the
  timestamp first. If it agrees with the timestamp recorded in the
  metadata, then `targets` considers the file unchanged. If the
  timestamps disagree, then `targets` recomputes the hash to make a
  final determination. This practice reduces the number of hash
  computations and thus saves time.

  However, timestamp precision varies from a few nanoseconds at best to
  2 entire seconds at worst, and timestamps with poor precision should
  not be fully trusted if there is any possibility that you will
  manually change the file within 2 seconds after the pipeline finishes.
  If the data store is on a file system with low-precision timestamps,
  then you may consider setting `trust_timestamps` to `FALSE` so
  `targets` errs on the safe side and always recomputes the hashes of
  files.

  To check if your file system has low-precision timestamps, you can run
  `file.create("x"); nanonext::msleep(1); file.create("y");` from within
  the directory containing the `_targets` data store and then check
  `difftime(file.mtime("y"), file.mtime("x"), units = "secs")`. If the
  value from [`difftime()`](https://rdrr.io/r/base/difftime.html) is
  around 0.001 seconds (must be strictly above 0 and below 1) then you
  do not need to set `trust_timestamps = FALSE`.

- trust_object_timestamps:

  Deprecated. Use `trust_timestamps` instead.

## Value

`NULL` (invisibly).

## Storage formats

`targets` has several built-in storage formats to control how return
values are saved and loaded from disk:

- `"rds"`: Default, uses
  [`saveRDS()`](https://rdrr.io/r/base/readRDS.html) and
  [`readRDS()`](https://rdrr.io/r/base/readRDS.html). Should work for
  most objects, but slow.

- `"auto"`: either `"file"` or `"qs"`, depending on the return value of
  the target. If the return value is a character vector of existing
  files (and/or directories), then the format becomes `"file"` before
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  saves the target. Otherwise, the format becomes `"qs"`.

  NOTE: `format = "auto"` slows down pipelines with 10000+ targets
  because it creates deep copies of 20000+ internal data objects.
  Pipelines of this size should use a more explicit format instead of
  `"auto"`.

- `"qs"`: Uses
  [`qs2::qs_save()`](https://rdrr.io/pkg/qs2/man/qs_save.html) and
  [`qs2::qs_read()`](https://rdrr.io/pkg/qs2/man/qs_read.html). Should
  work for most objects, much faster than `"rds"`. Optionally configure
  settings through
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  and
  [`tar_resources_qs()`](https://docs.ropensci.org/targets/reference/tar_resources_qs.md).

  Prior to `targets` version 1.8.0.9014, `format = "qs"` used the `qs`
  package. `qs` has since been superseded in favor of `qs2`, and so
  later versions of `targets` use `qs2` to save new data. To read
  existing data, `targets` first attempts
  [`qs2::qs_read()`](https://rdrr.io/pkg/qs2/man/qs_read.html), and then
  if that fails, it falls back on `qs::qread()`.

- `"feather"`: Uses `arrow::write_feather()` and `arrow::read_feather()`
  (version 2.0). Much faster than `"rds"`, but the value must be a data
  frame. Optionally set `compression` and `compression_level` in
  `arrow::write_feather()` through
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  and
  [`tar_resources_feather()`](https://docs.ropensci.org/targets/reference/tar_resources_feather.md).
  Requires the `arrow` package (not installed by default).

- `"parquet"`: Uses `arrow::write_parquet()` and `arrow::read_parquet()`
  (version 2.0). Much faster than `"rds"`, but the value must be a data
  frame. Optionally set `compression` and `compression_level` in
  `arrow::write_parquet()` through
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  and
  [`tar_resources_parquet()`](https://docs.ropensci.org/targets/reference/tar_resources_parquet.md).
  Requires the `arrow` package (not installed by default).

- `"fst"`: Uses
  [`fst::write_fst()`](http://www.fstpackage.org/reference/write_fst.md)
  and
  [`fst::read_fst()`](http://www.fstpackage.org/reference/write_fst.md).
  Much faster than `"rds"`, but the value must be a data frame.
  Optionally set the compression level for
  [`fst::write_fst()`](http://www.fstpackage.org/reference/write_fst.md)
  through
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  and
  [`tar_resources_fst()`](https://docs.ropensci.org/targets/reference/tar_resources_fst.md).
  Requires the `fst` package (not installed by default).

- `"fst_dt"`: Same as `"fst"`, but the value is a `data.table`. Deep
  copies are made as appropriate in order to protect against the global
  effects of in-place modification. Optionally set the compression level
  the same way as for `"fst"`.

- `"fst_tbl"`: Same as `"fst"`, but the value is a `tibble`. Optionally
  set the compression level the same way as for `"fst"`.

- `"keras"`: superseded by
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  and incompatible with `error = "null"` (in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  or `tar_option_set()`). Uses
  [`keras::save_model_hdf5()`](https://rdrr.io/pkg/keras/man/save_model_hdf5.html)
  and
  [`keras::load_model_hdf5()`](https://rdrr.io/pkg/keras/man/save_model_hdf5.html).
  The value must be a Keras model. Requires the `keras` package (not
  installed by default).

- `"torch"`: superseded by
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  and incompatible with `error = "null"` (in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  or `tar_option_set()`). Uses
  [`torch::torch_save()`](https://torch.mlverse.org/docs/reference/torch_save.html)
  and
  [`torch::torch_load()`](https://torch.mlverse.org/docs/reference/torch_load.html).
  The value must be an object from the `torch` package such as a tensor
  or neural network module. Requires the `torch` package (not installed
  by default).

- `"file"`: A file target. To use this format, the target needs to
  manually identify or save some data and return a character vector of
  paths to the data (must be a single file path if `repository` is not
  `"local"`). (These paths must be existing files and nonempty
  directories.) Then, `targets` automatically checks those files and
  cues the appropriate run/skip decisions if those files are out of
  date. Those paths must point to files or directories, and they must
  not contain characters `|` or `*`. All the files and directories you
  return must actually exist, or else `targets` will throw an error.
  (And if `storage` is `"worker"`, `targets` will first stall out trying
  to wait for the file to arrive over a network file system.) If the
  target does not create any files, the return value should be
  `character(0)`.

  If `repository` is not `"local"` and `format` is `"file"`, then the
  character vector returned by the target must be of length 1 and point
  to a single file. (Directories and vectors of multiple file paths are
  not supported for file targets on the cloud.) That output file is
  uploaded to the cloud and tracked for changes where it exists in the
  cloud. As of `targets` version \>= 1.11.0, the physical file is
  retained locally after the target runs. (Previous versions of
  `targets` deleted the file locally.)

- `"url"`: An input URL. For this storage format, `repository` is
  implicitly `"local"`, URL format is like `format = "file"` except the
  return value of the target is a URL that already exists and serves as
  input data for downstream targets. Optionally supply a custom `curl`
  handle through
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  and
  [`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md).
  in `new_handle()`, `nobody = TRUE` is important because it ensures
  `targets` just downloads the metadata instead of the entire data file
  when it checks time stamps and hashes. The data file at the URL needs
  to have an ETag or a Last-Modified time stamp, or else the target will
  throw an error because it cannot track the data. Also, use extreme
  caution when trying to use `format = "url"` to track uploads. You must
  be absolutely certain the ETag and Last-Modified time stamp are fully
  updated and available by the time the target's command finishes
  running. `targets` makes no attempt to wait for the web server.

- A custom format can be supplied with
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md).
  For this choice, it is the user's responsibility to provide methods
  for (un)serialization and (un)marshaling the return value of the
  target.

- The formats starting with `"aws_"` are deprecated as of 2022-03-13
  (`targets` version \> 0.10.0). For cloud storage integration, use the
  `repository` argument instead.

Formats `"rds"`, `"file"`, and `"url"` are general-purpose formats that
belong in the `targets` package itself. Going forward, any additional
formats should be implemented with
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
in third-party packages like `tarchetypes` and `geotargets` (for
example:
[`tarchetypes::tar_format_nanoparquet()`](https://docs.ropensci.org/tarchetypes/reference/tar_format_nanoparquet.html)).
Formats `"qs"`, `"fst"`, etc. are legacy formats from before the
existence of
[`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md),
and they will continue to remain in `targets` without deprecation.

## See also

Other configuration:
[`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
[`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md),
[`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md),
[`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md),
[`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md),
[`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md),
[`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
[`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md),
[`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md),
[`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)

## Examples

``` r
tar_option_get("format") # default format before we set anything
#> [1] "rds"
tar_target(x, 1)$settings$format
#> [1] "rds"
tar_option_set(format = "fst_tbl") # new default format
tar_option_get("format")
#> [1] "fst_tbl"
tar_target(x, 1)$settings$format
#> [1] "fst_tbl"
tar_option_reset() # reset the format
tar_target(x, 1)$settings$format
#> [1] "rds"
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
  library(targets)
  library(tarchetypes)
  tar_option_set(cue = tar_cue(mode = "always")) # All targets always run.
  list(tar_target(x, 1), tar_target(y, 2))
})
tar_make()
tar_make()
})
}
```
