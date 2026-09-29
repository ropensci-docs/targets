# Changelog

## targets 1.12.0

CRAN release: 2026-02-09

- Avoid loading non-targets in `tar_load(everything())`
  ([\#1529](https://github.com/ropensci/targets/issues/1529),
  [@pitakakariki](https://github.com/pitakakariki)).
- For internal consistency, return `NA_character_` from
  [`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md)
  when `format` is `"file"`
  ([\#1532](https://github.com/ropensci/targets/issues/1532),
  [@noamross](https://github.com/noamross)).
- Add
  [`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md)
  ([\#1521](https://github.com/ropensci/targets/issues/1521),
  [@noamross](https://github.com/noamross)).
- Use a new `TAR_ACTIVE` environment variable instead of a field of the
  `tar_runtime` object for `tar_runtime()`
  (<https://github.com/ropensci/tarchetypes/issues/232>,
  [@lgaborini](https://github.com/lgaborini)).
- Add
  [`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md)
  and recommend it along with
  [`igraph::find_cycle()`](https://r.igraph.org/reference/find_cycle.html)
  for debugging
  ([\#1562](https://github.com/ropensci/targets/issues/1562),
  [@tylermorganwall](https://github.com/tylermorganwall)).
- Avoid the unclean shutdown message in the `clustermq` multi-process
  scheduler by calling `cleanup()` repeatedly until it returns `TRUE`.
  Uses exponential backoff to avoid excessive CPU load.
- Use sequential controller in `covr`.
- Ensure hash stability of vector slices in R \>= 4.6.0
  ([\#1566](https://github.com/ropensci/targets/issues/1566)).
- Use a dummy send instead of `send_wait()` to work around
  <https://github.com/mschubert/clustermq/issues/340>
  ([\#1566](https://github.com/ropensci/targets/issues/1566)).
- Set `strict = TRUE` by default in
  [`tidyselect::eval_select()`](https://tidyselect.r-lib.org/reference/eval_select.html)
  to make `tidyselect` interfaces consistent with
  [`dplyr::select()`](https://dplyr.tidyverse.org/reference/select.html)
  (except the `exclude` argument of graph functions, which need to allow
  names to not exist in the list of choices)
  ([\#1563](https://github.com/ropensci/targets/issues/1563),
  [@statzhero](https://github.com/statzhero)).

## targets 1.11.4

CRAN release: 2025-09-13

- Tone down progress bar output for medium-overhead scenarios.
- Speed up
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md)
  default settings for
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  etc. (for million-target pipelines).
- Choose the `"terse"` reporter by default if the calling session is
  non-interactive. This will hopefully avoid problems on CRAN for
  packages that use `targets` with the default settings.
- Improve reporter deprecation messages
  ([\#1493](https://github.com/ropensci/targets/issues/1493),
  [@dakvid](https://github.com/dakvid)).
- Clarify scope of
  [`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md)
  ([\#1506](https://github.com/ropensci/targets/issues/1506),
  [@valentingar](https://github.com/valentingar)).
- Handle errors in
  [`rstudioapi::isAvailable()`](https://rstudio.github.io/rstudioapi/reference/isAvailable.html)
  ([\#1519](https://github.com/ropensci/targets/issues/1519),
  [@dipterix](https://github.com/dipterix)).
- Improve error message when metadata file is corrupted
  ([\#1523](https://github.com/ropensci/targets/issues/1523),
  [@dakvid](https://github.com/dakvid)).

## targets 1.11.3

CRAN release: 2025-05-08

### Bug fixes

- Use `qmethod = "escape"` to avoid
  <https://github.com/Rdatatable/data.table/issues/3509>
  ([\#1480](https://github.com/ropensci/targets/issues/1480),
  [@koefoeden](https://github.com/koefoeden)).
- Ensure `error = "trim"` does not hang when the errored target has a
  long chain of reverse dependencies
  ([\#1481](https://github.com/ropensci/targets/issues/1481),
  [@koefoeden](https://github.com/koefoeden)).
- Manually remove class `"rlib_error_package_not_found"` from errors
  ([\#1484](https://github.com/ropensci/targets/issues/1484),
  [@malcolmbarrett](https://github.com/malcolmbarrett)). This and
  [\#1354](https://github.com/ropensci/targets/issues/1354) are
  unfortunate consequences of
  [\#997](https://github.com/ropensci/targets/issues/997).

### Other changes

- Call
  [`suppressPackageStartupMessages()`](https://rdrr.io/r/base/message.html)
  once for the whole pipeline. Repeated target-specific calls may be
  slow, and the messages themselves are cumbersome. This is an
  appropriate tradeoff.
- Ensure the progress bar from the balanced reporter does not chop up
  messages from
  [`tar_debug_instructions()`](https://docs.ropensci.org/targets/reference/tar_debug_instructions.md).
- Remove ANSI escape sequences from warnings and error messages.
- Use [`cli::cli_text()`](https://cli.r-lib.org/reference/cli_text.html)
  instead of
  [`cli::cli_progress_output()`](https://cli.r-lib.org/reference/cli_progress_output.html)
  ([\#1478](https://github.com/ropensci/targets/issues/1478),
  [@dipterix](https://github.com/dipterix)).
- Minor speedups in the beginning and end of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  ([\#1482](https://github.com/ropensci/targets/issues/1482)).
- Cache `_targets/objects/` time stamps only for local builders
  mentioned in the metadata, as opposed to everything in that directory
  ([\#1482](https://github.com/ropensci/targets/issues/1482)).
- Instrument pre-processing overhead with progress bars
  ([\#1482](https://github.com/ropensci/targets/issues/1482)).

## targets 1.11.2

- Documentation fix: if `format` is `"file"` and `repository` is not
  `"local"`, then the local file is no longer deleted after upload
  ([\#1467](https://github.com/ropensci/targets/issues/1467)).
- Improve legend labels in graphs.
- Repair mermaid.js graphs with disconnected edges
  ([\#1472](https://github.com/ropensci/targets/issues/1472)).

## targets 1.11.1

CRAN release: 2025-04-10

- Bugfix: `rstudio_available()` returns `FALSE` without error if
  `rstudioapi` is not installed.
- Add a new `"terse"` reporter, which is the `"balanced"` reporter
  without the progress bar. Make `"terse"` the default reporter

## targets 1.11.0

### Deprecated features

- Deprecate the `priority` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).
  Because of [\#1458](https://github.com/ropensci/targets/issues/1458),
  custom priorities no longer have an effect on execution order.
  However, up-to-date parallelized pipelines with 100000+ targets can
  now be checked around 10 times faster, so the tradeoff is worth it.
  And as a workaround, you can send high-priority targets to one or more
  special `crew` controllers in a controller group (details:
  <https://books.ropensci.org/targets/crew.html#heterogeneous-workers>).

### Changes to default behavior

- Keep `format = "file"` files on disk even for non-local repositories
  ([\#1467](https://github.com/ropensci/targets/issues/1467)).

### Changes to default settings

- In
  [`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
  set `repository_meta` to `"local"` by default, regardless of
  `repository`
  ([\#1427](https://github.com/ropensci/targets/issues/1427)).
- In
  [`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md),
  set `storage = "worker"`, `retrieval = "auto"`, and `memory = "auto"`
  by default
  ([\#1426](https://github.com/ropensci/targets/issues/1426)). For
  `memory`, `"auto"` is now equivalent to `"transient"` most of the
  time, but it is equivalent to `"persistent"` for non-dynamic targets
  that other targets dynamically branch over. For `retrieval`, the
  `"auto"` setting is new. It is equivalent to `"worker"` for most
  cases, but it aligns with `"main"` for dynamic branches that branch
  over non-dynamic targets. All this is to avoid re-reading the upstream
  target from disk every time a branch needs to run.
- Set the new “balanced” reporter to be the default reporter for
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md).
- Set the default `garbage_collection` argument of
  [`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md)
  to 0 ([\#1464](https://github.com/ropensci/targets/issues/1464)).

### Efficiency improvements

- Speed up checking up-to-date targets in large dynamic branching
  pipelines ([\#1458](https://github.com/ropensci/targets/issues/1458),
  [\#1460](https://github.com/ropensci/targets/issues/1460)). The
  speedup is over 10-fold or more in some cases.
- Maintain a persistent text connection when appending to a metadata
  text file ([\#1415](https://github.com/ropensci/targets/issues/1415)).
- Avoid superfluous garbage collection when `crew` controllers are
  saturated.
- Set defaults for `storage`, `retrieval`, and `memory` that balance
  resource tradeoffs for the most common pipelines
  ([\#1426](https://github.com/ropensci/targets/issues/1426)).
- Garbage collection only runs in `targets:::target_run()`
  ([\#1464](https://github.com/ropensci/targets/issues/1464)). There is
  no longer a separate [`gc()`](https://rdrr.io/r/base/gc.html) call on
  the main process.
- Shave off overhead from `store_sync_file_meta()` in the general case.

### Other changes

- Upload workspaces to the cloud if `tar_option_get("repository_meta")`
  is `"aws"` or `"gcp"`. Download them with
  [`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md)
  and delete them with `tar_destroy(destroy = "all")` or
  `tar_destroy(destroy = "cloud")`.
- Deep-copy settings when resolving `format = "auto"`
  ([\#1425](https://github.com/ropensci/targets/issues/1425),
  [@paulseamer](https://github.com/paulseamer)).
- Add `store_read_path.tar_auto()`
  ([\#1429](https://github.com/ropensci/targets/issues/1429),
  [@paulseamer](https://github.com/paulseamer)).
- Improve error message that explains `iteration = "group"` branching
  problems.
- Allow more special characters in recorded warnings and error messages.
- Call
  [`cli::style_reset()`](https://cli.r-lib.org/reference/ansi-styles.html)
  at the end of non-silent reporters
  ([\#1450](https://github.com/ropensci/targets/issues/1450),
  [@r2evans](https://github.com/r2evans)).
- Exclude lists of target definitions from the globals in the dependency
  graph ([\#1431](https://github.com/ropensci/targets/issues/1431)).
- Nomenclature change: drop the term “dynamic file” in favor of “file
  target”.
- Internally choose a default level separation in the `visNetwork` graph
  based on the number of hierarchical levels and the maximum number of
  vertices per level
  ([\#1432](https://github.com/ropensci/targets/issues/1432)).
- In
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
  choose the colors of the edges based on the origin vertices, not the
  destination vertices
  ([\#1433](https://github.com/ropensci/targets/issues/1433)).
- In the `"verbose"` and `"timestamp"` reporters, print “dispatched
  pattern” messages, and print the total computation and storage summed
  over all the branches.
- Create a new `"balanced"` reporter with a `cli` progress bar
  ([\#1442](https://github.com/ropensci/targets/issues/1442)).
- Deprecate reporters `"forecast"`, `"forecast_interactive"`,
  `"verbose_positives"`, and `"timestamp_positives"`
  ([\#1442](https://github.com/ropensci/targets/issues/1442)).
- Ensure colors printed to the console are preserved when forwarded from
  the `callr` process
  ([\#1442](https://github.com/ropensci/targets/issues/1442)).
- Add
  [`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)
  (<https://github.com/ropensci/tarchetypes/issues/215>,
  [@noamross](https://github.com/noamross)).
- Use `prettyunits` to print elapsed times and file sizes.
- Shorten and simplify the
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  error message.
- Minor bugfix: add a new `on_worker` argument to `target_run()` and
  `builder_unload_value()` so the latter only removes the target value
  if the target was actually run on a worker.

## targets 1.10.1

CRAN release: 2025-01-31

- Restore explicit references to “self” in `R6` classes.
- Perform `crew` task retries.
- Try to handle `NA` buckets in `store_delete_objects.tar_aws()` and
  `store_delete_objects.tar_gcp()`.

## targets 1.10.0

CRAN release: 2025-01-13

### Invalidating changes

These changes invalidate certain targets in a pipeline and cause them to
rerun on the next
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).

- Exclude function signatures from
  [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
  output strings to reduce the size of pipeline metadata
  ([\#1390](https://github.com/ropensci/targets/issues/1390)).
- Exclude function signatures from
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  output strings to reduce the size of pipeline metadata
  ([\#1390](https://github.com/ropensci/targets/issues/1390)).

### Summary of performance gains

[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
and
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
run much faster in this release. Extensive profiling was done on a
real-world simulation pipeline with 66002 up-to-date targets. For
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
using all the default settings:

| Machine    | Before (seconds) | After (seconds) | Speedup  |
|------------|------------------|-----------------|----------|
| M2 Macbook | 413.16           | 35.538          | 11.62587 |
| RHEL9      | 450.66           | 94.08           | 4.790    |

And for
[`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
using all the default settings

| Machine    | Before (seconds) | After (seconds) | Speedup  |
|------------|------------------|-----------------|----------|
| M2 Macbook | 91.314           | 16.636          | 5.48894  |
| RHEL9      | 167.809          | 37.395          | 4.487472 |

To take advantage of these speed gains for an existing pipeline, you may
have to run
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
to convert the time stamps and file sizes to a new format. This initial
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
is slow, but subsequent
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
calls should be much faster than before the upgrade.

### Other/specific changes

- Speed up
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
  by avoiding excessive buffering and disk writes for metadata and
  reporters when the pipeline is just skipping targets.
- Use a more lookup-efficient data structure for `tar_runtime$file_info`
  ([\#1398](https://github.com/ropensci/targets/issues/1398)).
- Fall back on vector aggregation without names
  ([\#1401](https://github.com/ropensci/targets/issues/1401),
  [@guglicap](https://github.com/guglicap)).
- Speed up representation of file sizes in metadata
  ([\#1408](https://github.com/ropensci/targets/issues/1408)).
- Add a new `"forecast_interactive"` reporter to
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
  to choose `"forecast"` for interactive sessions and `"silent"` for
  non-interactive ones.
- Add a new `seconds_reporter_outdated` argument to
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  with a default of 1 to control the time interval of the reporter of
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
  and other passive algorithm functions.
- Remove target descriptions from the default labels of graph
  visualizations.

## targets 1.9.1

CRAN release: 2024-12-04

### Bug fixes

- Allow branch references to contain multi-element `path` vectors with
  cloud metadata
  ([\#1382](https://github.com/ropensci/targets/issues/1382),
  [@n8layman](https://github.com/n8layman)).
- Avoid partial matches in internal code
  ([\#1384](https://github.com/ropensci/targets/issues/1384),
  [@olivroy](https://github.com/olivroy)).
- Add error handling around calls to
  [`ps::ps_disk_partitions()`](https://ps.r-lib.org/reference/ps_disk_partitions.html)
  and
  [`ps::ps_fs_mount_point()`](https://ps.r-lib.org/reference/ps_fs_mount_point.html).
- Do not store `_targets/objects/` paths in metadata for CAS
  repositories
  ([\#1391](https://github.com/ropensci/targets/issues/1391)).

### Compatibility

- Ensure compatibility with `igraph` \>= 2.1.2.

## targets 1.9.0

CRAN release: 2024-11-20

### Improvements

- Un-break workflows that use `format = "file_fast"`
  ([\#1339](https://github.com/ropensci/targets/issues/1339),
  [@koefoeden](https://github.com/koefoeden)).
- Fix deadlock in `error = "trim"`
  ([\#1340](https://github.com/ropensci/targets/issues/1340),
  [@koefoeden](https://github.com/koefoeden)).
- Remove tailored debugging message
  ([\#1341](https://github.com/ropensci/targets/issues/1341),
  [@koefoeden](https://github.com/koefoeden)).
- Store warnings while writing to storage
  ([\#1345](https://github.com/ropensci/targets/issues/1345),
  [@Aariq](https://github.com/Aariq)).
- Allow `garbage_collection` to be a non-negative integer to control the
  frequency of garbage collection in a performant, convenient, unified
  way ([\#1351](https://github.com/ropensci/targets/issues/1351)).
- Deprecate the `garbage_collection` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
  and `tar_make_clusterm()`
  ([\#1351](https://github.com/ropensci/targets/issues/1351)).
- Instrument `target_run()`, `target_prepare()`, and `target_conclude()`
  using `autometric`.
- Avoid sending problematic error classes such as
  `"vctrs_error_subscript_oob"` to
  [`rlang::abort()`](https://rlang.r-lib.org/reference/abort.html)
  ([\#1354](https://github.com/ropensci/targets/issues/1354),
  [@Jiefei-Wang](https://github.com/Jiefei-Wang)).
- Reduce memory consumption by ~23% in large pipelines by avoiding the
  accumulation of promise objects
  ([\#1352](https://github.com/ropensci/targets/issues/1352)).
- Avoid `store_assert_format()` and `store_convert_object()` is
  `storage` is `"none"`.
- Add a [`list()`](https://rdrr.io/r/base/list.html) method to
  [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
  to make it easier and more efficient to specify custom CAS
  repositories
  ([\#1366](https://github.com/ropensci/targets/issues/1366)).
- Improve speed and reduce memory consumption by avoiding deep copies of
  inner environments of target definition objects
  ([\#1368](https://github.com/ropensci/targets/issues/1368)).
- Reduce memory consumption by storing buds and branches as lightweight
  references when `memory` is `"transient"`
  ([\#1364](https://github.com/ropensci/targets/issues/1364)).
- Replace the `memory` class with the new `lookup` class.
- Implement `memory = "auto"` to select transient memory for dynamic
  branches and persistent memory for other targets
  ([\#1371](https://github.com/ropensci/targets/issues/1371)).
- Omit whole pattern targets from branch subpipelines when possible.
  Should reduce memory consumption in some cases.
- Omit whole stem targets from branch subpipelines when `retrieval` is
  `"main"` and only a bud is actually used. The same cannot be done with
  branches because each branch may need to be (un)marshaled
  individually.
- Compress branches into references when `retrieval` is `"worker"` and
  the whole pattern is part of the subpipeline.
- Avoid duplicated branch aggregation: just send the branches over the
  network.
- Back-compatibly switch `format = "qs"` from `qs` to `qs2`
  ([\#1373](https://github.com/ropensci/targets/issues/1373)).
- Add
  [`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md).

### Potentially invalidating changes

- Add `"keepNA"` and `"keepInteger"` to
  [`.deparseOpts()`](https://rdrr.io/r/base/deparseOpts.html)
  ([\#1375](https://github.com/ropensci/targets/issues/1375)). This may
  cause existing pipelines to rerun, but it makes add-ons like
  [`tarchetypes::tar_map()`](https://docs.ropensci.org/tarchetypes/reference/tar_map.html)
  much easier to use.

## targets 1.8.0

CRAN release: 2024-10-02

- Wrap
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  UI module in
  [`bslib::page()`](https://rstudio.github.io/bslib/reference/page.html)
  ([\#1302](https://github.com/ropensci/targets/issues/1302),
  [@kwbyron-lilly](https://github.com/kwbyron-lilly)).
- Remove `callr_function` in `tar_make_as_job()` argument list.
- Ensure `storage = "worker"` is respected when the process of storing
  an object generates an error
  ([\#1304](https://github.com/ropensci/targets/issues/1304),
  [@multimeric](https://github.com/multimeric)).
- Default to the `_targets.R` pattern in
  [`tar_branches()`](https://docs.ropensci.org/targets/reference/tar_branches.md)
  ([\#1306](https://github.com/ropensci/targets/issues/1306),
  [@multimeric](https://github.com/multimeric),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Remove superfluous functions and globals from metadata with
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
  ([\#1312](https://github.com/ropensci/targets/issues/1312),
  [@benzipperer](https://github.com/benzipperer)).
- Change the default `workspace_on_error` option to `TRUE`
  ([\#1310](https://github.com/ropensci/targets/issues/1310),
  [@hadley](https://github.com/hadley)).
- Enhance and organize the `error = "stop"` error message.
- Avoid saving a file in `_targets/objects` for `error = "null"`.
  Instead, switch to a special `"null"` storage format class if `error`
  is `"null"` the target throws an error. This should allow users to
  more freely create new formats with
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  without worrying about how to handle `NULL` objects created by
  `error = "null"`.
- Implement `format = "auto"`
  ([\#1311](https://github.com/ropensci/targets/issues/1311),
  [@hadley](https://github.com/hadley)).
- Replace `pingr` dependency with
  [`base::socketConnection()`](https://rdrr.io/r/base/connections.html)
  for local URL utilities
  ([\#1317](https://github.com/ropensci/targets/issues/1317),
  [\#1318](https://github.com/ropensci/targets/issues/1318),
  [@Adafede](https://github.com/Adafede)).
- Implement
  [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md),
  [`tar_repository_cas_local()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local.md),
  and
  [`tar_repository_cas_local_gc()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local_gc.md)
  for content-addressable storage
  ([\#1232](https://github.com/ropensci/targets/issues/1232),
  [\#1314](https://github.com/ropensci/targets/issues/1314),
  [@noamross](https://github.com/noamross)).
- Add
  [`tar_format_get()`](https://docs.ropensci.org/targets/reference/tar_format_get.md)
  to make implementing CAS systems easier.
- Implement `error = "trim"` in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  and
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  ([\#1310](https://github.com/ropensci/targets/issues/1310),
  [\#1311](https://github.com/ropensci/targets/issues/1311),
  [@hadley](https://github.com/hadley)).
- Use the file system type to decide whether to trust time stamps
  ([\#1315](https://github.com/ropensci/targets/issues/1315),
  [@hadley](https://github.com/hadley),
  [@gaborcsardi](https://github.com/gaborcsardi)).
- Deprecate `format = "file_fast"` in favor of the above
  ([\#1315](https://github.com/ropensci/targets/issues/1315)).
- Deprecate `trust_object_timestamps` in favor of the more unified
  `trust_timestamps` in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  ([\#1315](https://github.com/ropensci/targets/issues/1315)).
- Print storage size of each target in verbose reporters
  ([\#1337](https://github.com/ropensci/targets/issues/1337),
  [@psychelzh](https://github.com/psychelzh)).
- Combine help files of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  and
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md).
  Same with
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  and
  [`tar_load_raw()`](https://docs.ropensci.org/targets/reference/tar_load.md).
- Add a `substitute` argument to
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  to make it easier to write custom storage formats without
  metaprogramming.

## targets 1.7.1

CRAN release: 2024-06-20

- Use `bslib` in
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md).
- Speed up `target_upstream_edges()` and `pipeline_upstream_edges()` by
  avoiding data frames until the last minute (17% speedup for certain
  kinds of large pipelines).
- Automatically set `as_job` to `FALSE` in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  if `rstudioapi` and/or RStudio is not available.

## targets 1.7.0

CRAN release: 2024-04-17

### Invalidating changes

- Use
  [`secretbase::siphash13()`](https://shikokuchuo.net/secretbase/reference/siphash13.html)
  instead of `digest(algo = "xxhash64", serializationVersion = 3)` so
  hashes of in-memory objects no longer depend on serialization version
  3 headers ([\#1244](https://github.com/ropensci/targets/issues/1244),
  [@shikokuchuo](https://github.com/shikokuchuo)). Unfortunately,
  pipelines built with earlier versions of `targets` will need to rerun.

### Other improvements

- Ensure patterns marshal properly
  ([\#1266](https://github.com/ropensci/targets/issues/1266),
  [\#1264](https://github.com/ropensci/targets/issues/1264),
  <https://github.com/njtierney/geotargets/issues/52>,
  [@Aariq](https://github.com/Aariq),
  [@njtierney](https://github.com/njtierney)).
- Inform and prompt the user when the pipeline was built with an old
  version of `targets` and changes to the package will cause the current
  work to rerun
  ([\#1244](https://github.com/ropensci/targets/issues/1244)). For the
  `tar_make*()` functions,
  [`utils::menu()`](https://rdrr.io/r/utils/menu.html) prompts the user
  to give people a chance to downgrade if necessary.
- For type safety in the internal database class, read all columns as
  character vectors in
  [`data.table::fread()`](https://rdrr.io/pkg/data.table/man/fread.html),
  then convert them to the correct types afterwards.
- Add a new
  [`tar_resources_custom_format()`](https://docs.ropensci.org/targets/reference/tar_resources_custom_format.md)
  function which can pass environment variables to customize the
  behavior of custom
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  storage formats
  ([\#1263](https://github.com/ropensci/targets/issues/1263),
  [\#1232](https://github.com/ropensci/targets/issues/1232),
  [@Aariq](https://github.com/Aariq),
  [@noamross](https://github.com/noamross)).
- Only marshal dependencies if actually sending the target to a parallel
  worker.

## targets 1.6.0

CRAN release: 2024-03-13

- Modernize `extras` in
  [`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md).
- [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  gains a `description` argument for free-form text describing what the
  target is about
  ([\#1230](https://github.com/ropensci/targets/issues/1230),
  [\#1235](https://github.com/ropensci/targets/issues/1235),
  [\#1236](https://github.com/ropensci/targets/issues/1236),
  [@tjmahr](https://github.com/tjmahr)).
- [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md),
  [`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md),
  [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md),
  and
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
  now optionally show target descriptions
  ([\#1230](https://github.com/ropensci/targets/issues/1230),
  [\#1235](https://github.com/ropensci/targets/issues/1235),
  [\#1236](https://github.com/ropensci/targets/issues/1236),
  [@tjmahr](https://github.com/tjmahr)).
- [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  is a new wrapper around
  [`tidyselect::any_of()`](https://tidyselect.r-lib.org/reference/all_of.html)
  to select specific subsets of targets based on the description rather
  than the name
  ([\#1136](https://github.com/ropensci/targets/issues/1136),
  [\#1196](https://github.com/ropensci/targets/issues/1196),
  [@noamross](https://github.com/noamross),
  [@mattmoo](https://github.com/mattmoo)).
- Fix the documentation of the `names` argument (nudge users toward
  `tidyselect` expressions).
- Make assertions on the pipeline process more robust (to check if two
  processes are trying to access the same data store).

## targets 1.5.1

CRAN release: 2024-02-15

- Avoid `arrow`-related CRAN check NOTE.
- [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  only writes the `_targets.R` script. The `run.sh` and `run.R` scripts
  are superseded by the `as_job` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
  Users not using the RStudio IDE can call
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  with `callr_function = callr::r_bg` to run the pipeline as a
  background process.
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  are superseded in favor `tar_make(use_crew = TRUE)`, so template files
  are no longer written for the former automatically.

## targets 1.5.0

### Invalidating changes

Because of the changes below, upgrading to this version of `targets`
will unavoidably invalidate previously built targets in existing
pipelines. Your pipeline code should still work, but any targets you ran
before will most likely need to rerun after the upgrade.

- In
  [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md),
  use `secretbase::sha3(x = TARGET_NAME, bits = 32L, convert = NA)` to
  generate target seeds that are more resistant to overlapping RNG
  streams ([\#1139](https://github.com/ropensci/targets/issues/1139),
  [@shikokuchuo](https://github.com/shikokuchuo)). The previous approach
  used a less rigorous combination of `digest::digest(algo = "sha512")`
  and `digets::digest2int()`.

### Other improvements

- Update the documentation of the `deployment` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  to reflect the advent of `crew`
  ([\#1208](https://github.com/ropensci/targets/issues/1208),
  [@psychelzh](https://github.com/psychelzh)).
- Unset `cli.num_colors` on exit in
  [`tar_error()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  and
  [`tar_warning()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  ([\#1210](https://github.com/ropensci/targets/issues/1210),
  [@dipterix](https://github.com/dipterix)).
- Do not try to access `seconds_timeout` if the `crew` controller is
  actually a controller group
  ([\#1207](https://github.com/ropensci/targets/issues/1207),
  <https://github.com/wlandau/crew.cluster/discussions/35>,
  [@stemangiola](https://github.com/stemangiola),
  [@drejom](https://github.com/drejom)).
- [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  gains an `as_job` argument to optionally run a `targets` pipeline as
  an RStudio job.
- Bump required `igraph` version to 2.0.0 because
  [`igraph::get.edgelist()`](https://r.igraph.org/reference/get.edgelist.html)
  was deprecated in favor of
  [`igraph::as_edgelist()`](https://r.igraph.org/reference/as_edgelist.html).
- Do not dispatch targets to backlogged `crew` controllers (or
  controller groups)
  ([\#1220](https://github.com/ropensci/targets/issues/1220)). Use the
  new `push_backlog()` and `pop_backlog()` `crew` methods to make this
  smooth.
- Make the debugger message more generic
  ([\#1223](https://github.com/ropensci/targets/issues/1223),
  [@eliocamp](https://github.com/eliocamp)).
- Throw an early and informative error from
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  if there is already a `targets` pipeline running on a local process on
  the same local data store. The local process is detected using the
  process ID and time stamp from
  [`tar_process()`](https://docs.ropensci.org/targets/reference/tar_process.md)
  (with a 1.01-second tolerance for the time stamp).
- Remove
  [`pkgload::load_all()`](https://pkgload.r-lib.org/reference/load_all.html)
  warning ([\#1218](https://github.com/ropensci/targets/issues/1218)).
  Tried using `.__DEVTOOLS__` but it interferes with reverse
  dependencies.
- Add documentation and an assertion in
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  to let users know that `iteration = "group"` is invalid for dynamic
  targets (ones with `pattern = map(...)` etc.;
  [\#1226](https://github.com/ropensci/targets/issues/1226),
  [@bmfazio](https://github.com/bmfazio)).

## targets 1.4.1

CRAN release: 2024-01-09

- Print “errored pipeline” when at least one target errors.
- Bump minimum `clustermq` version to 0.9.2.
- Repair the
  [`tar_debug_instructions()`](https://docs.ropensci.org/targets/reference/tar_debug_instructions.md)
  tips for when commands are long.
- Do not look for dependencies of primitive functions
  ([\#1200](https://github.com/ropensci/targets/issues/1200),
  [@smwindecker](https://github.com/smwindecker),
  [@joelnitta](https://github.com/joelnitta)).

## targets 1.4.0

CRAN release: 2023-12-11

### Invalidating changes

Because of the changes below, upgrading to this version of `targets`
will unavoidably invalidate previously built targets in existing
pipelines. Your pipeline code should still work, but any targets you ran
before will most likely need to rerun after the upgrade.

- Use SHA512 during the creation of target-specific pseudo-random number
  generator seeds
  ([\#1139](https://github.com/ropensci/targets/issues/1139)). This
  change decreases the risk of overlapping/correlated random number
  generator streams. See the “RNG overlap” section of the
  [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md)
  help file for details and justification. Unfortunately, this change
  will invalidate all currently built targets because the seeds will be
  different. To avoid rerunning your whole pipeline, set
  `cue = tar_cue(seed = FALSE)` in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).
- For cloud storage: instead of the hash of the local file, use the ETag
  for AWS S3 targets and the MD5 hash for GCP GCS targets
  ([\#1172](https://github.com/ropensci/targets/issues/1172)). Sanitize
  with `targets:::digest_chr64()` in both cases before storing the
  result in the metadata.
- For a cloud target to be truly up to date, the hash in the metadata
  now needs to match the *current* object in the bucket, not the version
  recorded in the metadata
  ([\#1172](https://github.com/ropensci/targets/issues/1172)). In other
  words, `targets` now tries to ensure that the up-to-date data objects
  in the cloud are in their newest versions. So if you roll back the
  metadata to an older version, you will still be able to access
  historical data versions with
  e.g. [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md),
  but the pipeline will no longer be up to date.

### Other changes to seeds

- Add a new exported function
  [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md)
  which creates target-specific pseudo-random number generator seeds.
- Add an “RNG overlap” section in the
  [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md)
  help file to justify and defend how `targets` and `tarchetypes`
  approach pseudo-random numbers.
- Add function
  [`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md)
  which sets a seed and sets all the RNG algorithms to their defaults in
  the R installation of the user. Each target now uses
  [`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md)
  function to set its seed before running its R command
  ([\#1139](https://github.com/ropensci/targets/issues/1139)).
- Deprecate
  [`tar_seed()`](https://docs.ropensci.org/targets/reference/tar_seed.md)
  in favor of the new
  [`tar_seed_get()`](https://docs.ropensci.org/targets/reference/tar_seed_get.md)
  function.

### Other cloud storage improvements

- For all cloud targets, check hashes in batched LIST requests instead
  of individual HEAD requests
  ([\#1172](https://github.com/ropensci/targets/issues/1172)).
  Dramatically speeds up the process of checking if cloud targets are up
  to date.
- For AWS S3 targets,
  [`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
  and
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
  now use efficient batched calls to `delete_objects()` instead of
  costly individual calls to `delete_object()`
  ([\#1171](https://github.com/ropensci/targets/issues/1171)).
- Add a new `verbose` argument to
  [`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
  and
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md).
- Add a new `batch_size` argument to
  [`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
  and
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md).
- Add new arguments `page_size` and `verbose` to
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  ([\#1172](https://github.com/ropensci/targets/issues/1172)).
- Add a new
  [`tar_unversion()`](https://docs.ropensci.org/targets/reference/tar_unversion.md)
  function to remove version IDs from the metadata of cloud targets.
  This makes it easier to interact with just the current version of each
  target, as opposed to the version ID recorded in the local metadata.

### Other improvements

- Migrate to the changes in `clustermq` 0.9.0
  ([@mschubert](https://github.com/mschubert)).
- In progress statuses, change “started” to “dispatched” and change
  “built” to “completed”
  ([\#1192](https://github.com/ropensci/targets/issues/1192)).
- Deprecate
  [`tar_started()`](https://docs.ropensci.org/targets/reference/tar_started.md)
  in favor of
  [`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md)
  ([\#1192](https://github.com/ropensci/targets/issues/1192)).
- Deprecate
  [`tar_built()`](https://docs.ropensci.org/targets/reference/tar_built.md)
  in favor of
  [`tar_completed()`](https://docs.ropensci.org/targets/reference/tar_completed.md)
  ([\#1192](https://github.com/ropensci/targets/issues/1192)).
- Console messages from reporters say “dispatched” and “completed”
  instead of “started” and “built”
  ([\#1192](https://github.com/ropensci/targets/issues/1192)).
- The `crew` scheduling algorithm no longer waits on saturated
  controllers, and targets that are ready are greedily dispatched to
  `crew` even if all workers are busy
  ([\#1182](https://github.com/ropensci/targets/issues/1182),
  [\#1192](https://github.com/ropensci/targets/issues/1192)). To
  appropriately set expectations for users, reporters print “dispatched
  (pending)” instead of “dispatched” if the task load is backlogged at
  the moment.
- In the `crew` scheduling algorithm, waiting for tasks is now a truly
  event-driven process and consumes 5-10x less CPU resources
  ([\#1183](https://github.com/ropensci/targets/issues/1183)). Only the
  auto-scaling of workers uses polling (with an inexpensive default
  polling interval of 0.5 seconds, configurable through
  `seconds_interval` in the controller).
- Simplify stored target tracebacks.
- Print the traceback on error.

## targets 1.3.2

CRAN release: 2023-10-12

- Try to fix function help files for CRAN.

## targets 1.3.1

- Add
  [`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md)
  and
  [`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md)
  ([\#1153](https://github.com/ropensci/targets/issues/1153),
  [@psychelzh](https://github.com/psychelzh)).
- Apply error modes to `builder_wait_correct_hash()` in
  `target_conclude.tar_builder()`
  ([\#1154](https://github.com/ropensci/targets/issues/1154),
  [@gadenbuie](https://github.com/gadenbuie)).
- Remove duplicated error message from `builder_error_null()`.
- Allow
  [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md)
  and
  [`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md)
  to avoid errors if one or more metadata files do not exist. Add a new
  argument `strict` to control error behavior.
- Add new arguments `meta`, `progress`, `process`, and `crew` to control
  individual metadata files in
  [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md),
  [`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md),
  [`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md),
  and
  [`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md).
- Avoid newly deprecated arguments and functions in `crew` 0.5.0.9003
  (<https://github.com/wlnadau/crew/issues/131>).
- Allow
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  etc. inside a pipeline whenever it uses a different data store
  ([\#1158](https://github.com/ropensci/targets/issues/1158),
  [@MilesMcBain](https://github.com/MilesMcBain)).
- Set `seed = FALSE` in
  [`future::future()`](https://future.futureverse.org/reference/future.html)
  ([\#1166](https://github.com/ropensci/targets/issues/1166),
  [@svraka](https://github.com/svraka)).
- Add a new `physics` argument to
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  and
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  ([\#925](https://github.com/ropensci/targets/issues/925),
  [@Bdblodgett-usgs](https://github.com/Bdblodgett-usgs)).

## targets 1.3.0

CRAN release: 2023-09-11

### Invalidating changes

Because of these changes, upgrading to this version of `targets` will
unavoidably invalidate previously built targets in existing pipelines.
Your pipeline code should still work, but any targets you ran before
will most likely need to rerun after the upgrade.

- In the `hash_deps()` method of the metadata class, exclude symbols
  which are not actually dependencies, rather than just giving them
  empty strings. This change decouples the dependency hash from the hash
  of the target’s command
  ([\#1108](https://github.com/ropensci/targets/issues/1108)).

### Cloud metadata

- Continuously upload metadata files to the cloud during
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  ([\#1109](https://github.com/ropensci/targets/issues/1109)). Upload
  them to the repository specified in the `repository_meta`
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  option, and use the bucket and prefix set in the `resources`
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  option. `repository_meta` defaults to the existing `repository`
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  option.
- Add new functions
  [`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md),
  [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md),
  [`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md),
  and
  [`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md)
  to directly manage cloud metadata outside the pipeline
  ([\#1109](https://github.com/ropensci/targets/issues/1109)).

### Other changes

- Fix solution of
  [\#1103](https://github.com/ropensci/targets/issues/1103) so the copy
  fallback actually runs ([@jds485](https://github.com/jds485),
  [\#1102](https://github.com/ropensci/targets/issues/1102),
  [\#1103](https://github.com/ropensci/targets/issues/1103)).
- Switch back to [`tempdir()`](https://rdrr.io/r/base/tempfile.html) for
  [\#1103](https://github.com/ropensci/targets/issues/1103).
- Move `path_scratch_dir_network()` to `file.path(tempdir(), "targets")`
  and make sure `tar_destroy("all")` and `tar_destroy("cloud")` delete
  it.
- Display
  [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)
  subgraphs with transparent fills and black borders.
- Allow `database$get_data()` to work with list columns.
- Disallow functions that access the local data store (including
  metadata) from inside a target while the pipeline is running
  ([\#1055](https://github.com/ropensci/targets/issues/1055),
  [\#1063](https://github.com/ropensci/targets/issues/1063)). The only
  exception to this is local file targets such as `tarchetypes` literate
  programming target factories like
  [`tar_render()`](https://docs.ropensci.org/tarchetypes/reference/tar_render.html)
  and
  [`tar_quarto()`](https://docs.ropensci.org/tarchetypes/reference/tar_quarto.html).
- In the `hash_deps()` method of the metadata class, use a new custom
  `sort_chr()` function which temporarily sets the `LC_COLLATE` locale
  to `"C"` for sorting. This ensures lexicographic comparisons are
  consistent across platforms
  ([\#1108](https://github.com/ropensci/targets/issues/1108)).
- In
  [`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
  use the `file` argument and `keep.source = TRUE` to help with
  interactive debugging
  ([\#1120](https://github.com/ropensci/targets/issues/1120)).
- Deprecated `seconds_interval` in
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md),
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md).
  Replace it with `seconds_meta` (to control how often metadata gets
  saved) and `seconds_reporter` (to control how often to print messages
  to the R console)
  ([\#1119](https://github.com/ropensci/targets/issues/1119)).
- Respect `seconds_meta` and `seconds_reporter` for writing metadata and
  console messages even for currently building targets
  ([\#1055](https://github.com/ropensci/targets/issues/1055)).
- Retry all cloud REST API calls with HTTP error codes (429, 500-599)
  with the exponential backoff algorithm from `googleAuthR`
  ([\#1112](https://github.com/ropensci/targets/issues/1112)).
- For `format = "url"`, only retry on the HTTP error codes above.
- Make cloud temp file instances unique in order to avoid file conflicts
  with the same target.
- Un-deprecate `seconds_interval` and `seconds_timeout` from
  [`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md),
  and implement `max_tries` arguments in
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  and
  [`tar_resources_gcp()`](https://docs.ropensci.org/targets/reference/tar_resources_gcp.md)
  ([\#1127](https://github.com/ropensci/targets/issues/1127)).
- Use `file` and `keep.source` in
  [`parse()`](https://rdrr.io/r/base/parse.html) in `callr` utils and
  target Markdown.
- Automatically convert `"file_fast"` format to `"file"` format for
  cloud targets.
- In
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
  and
  [`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
  do not try to delete pattern targets which have no cloud storage.
- Add new arguments `seconds_timeout`, `close_connection`,
  `s3_force_path_style` to
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  to support the analogous arguments in
  [`paws.storage::s3()`](https://paws-r.r-universe.dev/paws.storage/reference/s3.html)
  ([\#1134](https://github.com/ropensci/targets/issues/1134),
  [@snowpong](https://github.com/snowpong)).

## targets 1.2.2

CRAN release: 2023-08-10

- Fix a documentation issue for CRAN.

## targets 1.2.1

- Add
  [`tar_prune_list()`](https://docs.ropensci.org/targets/reference/tar_prune_list.md)
  ([\#1090](https://github.com/ropensci/targets/issues/1090),
  [@mglev1n](https://github.com/mglev1n)).
- Wrap [`file.rename()`](https://rdrr.io/r/base/files.html) in
  [`tryCatch()`](https://rdrr.io/r/base/conditions.html) and fall back
  on a copy-then-remove workaround
  ([@jds485](https://github.com/jds485),
  [\#1102](https://github.com/ropensci/targets/issues/1102),
  [\#1103](https://github.com/ropensci/targets/issues/1103)).
- Stage temporary cloud upload/download files in
  `tools::R_user_dir(package = "targets", which = "cache")` instead of
  [`tempdir()`](https://rdrr.io/r/base/tempfile.html).
  `tar_destroy(destroy = "cloud")` and `tar_destroy(destroy = "all")`
  remove any leftover files from failed uploads/downloads
  ([@jds485](https://github.com/jds485),
  [\#1102](https://github.com/ropensci/targets/issues/1102),
  [\#1103](https://github.com/ropensci/targets/issues/1103)).
- Use `paws.storage` instead of all of `paws`.

## targets 1.2.0

CRAN release: 2023-06-26

### `crew` integration

- Do not assume S3 classes when validating `crew` controllers.
- Suggest a crew controller in the `_targets.R` file from
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md).
- Make
  [`tar_crew()`](https://docs.ropensci.org/targets/reference/tar_crew.md)
  compatible with `crew` \>= 0.3.0.
- Rename argument `terminate` to `terminate_controller` in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
- Add argument `use_crew` in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and add an option in
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  to make it configurable.
- Write progress data and metadata in `target_prepare()`.

### Other improvements

- Allow users to set the default `label` and `level_separation`
  arguments through
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  ([\#1085](https://github.com/ropensci/targets/issues/1085),
  [@Moohan](https://github.com/Moohan)).

## targets 1.1.3

CRAN release: 2023-05-23

- Decide on `nanonext` usage in `time_seconds_local()` at runtime and
  not installation time. That way, if `nanonext` is removed after
  `targets` is installed, functions in `targets` still work. Fixes the
  CRAN issues seen in `tarchetypes`, `jagstargets`, and `gittargets`.

## targets 1.1.2

CRAN release: 2023-05-23

- Remove `crew`-related startup messages.

## targets 1.1.1

- Pre-compute `cli` colors and bullets to improve performance in
  RStudio.
- Use [`packageStartupMessage()`](https://rdrr.io/r/base/message.html)
  for package startup messages.

## targets 1.1.0

### Bug fixes

- Send targets to the appropriate controller in a controller group when
  `crew` is used.

### General improvements

- Call [`gc()`](https://rdrr.io/r/base/gc.html) more appropriately when
  `garbage_collection` is `TRUE` in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).
- Add `garbage_collection` arguments to
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  to add optional garbage collection before targets are sent to workers.
  This is different and independent from the `garbage_collection`
  argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md).
  In high-performance computing scenarios, the former controls what
  happens on the main controlling process, whereas the latter controls
  what happens on the worker.
- Add `garbage_collection` and `seconds_interval` arguments to
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md).
- Downsize the `tar_runtime` object.
- Remove the 100 Kb file size cutoff for determining whether to trust
  the file timestamp or recompute the hash when checking if a file is up
  to date ([\#1062](https://github.com/ropensci/targets/issues/1062)).
  Instate the `"file_fast"` format and the `trust_object_timestamps`
  option in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  as safer alternatives.
- Consolidate store constructors.
- Allow `crew` controller groups
  ([\#1065](https://github.com/ropensci/targets/issues/1065),
  [@mglev1n](https://github.com/mglev1n)).
- Expose more exponential backoff configuration parameters through
  [`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md).
  The `backoff` argument of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  now accepts output from
  [`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
  and supplying a numeric is deprecated.
- Fix the exponential backoff rules in the `crew` scheduling algorithm.
- Implement
  [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md)
  to configure retries and timeouts for internal HTTP/HTTPS requests in
  specialized targets with `format = "url"`, `repository = "aws"`, and
  `repository = "gcp"`. Also applies to syncing target files across
  network file systems in the case of `storage = "worker"` or
  `format = "file"`, which previously had a hard-coded
  `seconds_interval = 0.1` and `seconds_timeout = 60`.
- Deprecate `seconds_interval` and `seconds_timeout` in
  [`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md)
  in favor of the new equivalent arguments of
  [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md)
- Safely withhold a target from its `crew` controller when the
  controller is saturated
  ([\#1074](https://github.com/ropensci/targets/issues/1074),
  [@mglev1n](https://github.com/mglev1n)).
- Use exponential backoff when appending a target back to the queue in
  the case of a saturated `crew` controller.
- Use native retries in `paws.common`
  ([@DyfanJones](https://github.com/DyfanJones)).

### Speedups

- Cache info about all of `_targets/objects/` in
  [`tar_callr_inner_try()`](https://docs.ropensci.org/targets/reference/tar_callr_inner_try.md)
  and update the cache as targets are saved to `_targets/objects/` to
  avoid the overhead of repeated calls to
  [`file.exists()`](https://rdrr.io/r/base/files.html) and
  [`file.info()`](https://rdrr.io/r/base/file.info.html)
  ([\#1056](https://github.com/ropensci/targets/issues/1056)).
- Trust the timestamps by default when checking whether files in
  `_targets/objects/` are up to date
  ([\#1062](https://github.com/ropensci/targets/issues/1062)).
  `tar_option_set(trust_object_timestamps = FALSE)` ignores the
  timestamps and recomputes the hashes.
- Write to `_targets/meta/meta` and `_targets/meta/progress` in timed
  batches instead of line by line
  ([\#1055](https://github.com/ropensci/targets/issues/1055)).
- Reporters now print progress messages in timed batches instead of line
  by line ([\#1055](https://github.com/ropensci/targets/issues/1055)).
- The summary and forecast reporters are much faster because they avoid
  going through data frames.
- Avoid [`tempfile()`](https://rdrr.io/r/base/tempfile.html) when
  working with the scratch directory.
- Use
  [`nanonext::mclock()`](https://nanonext.r-lib.org/reference/mclock.html)
  instead of [`proc.time()`](https://rdrr.io/r/base/proc.time.html) when
  there is no risk of forked processes.
- Replace `withr` with slightly faster/leaner base R alternatives.
- Efficiently catch changes to the working directory instead of
  overburdening the pipeline with calls to
  [`setwd()`](https://rdrr.io/r/base/getwd.html)
  ([\#1057](https://github.com/ropensci/targets/issues/1057)).
- Invoke `tar_options` methods in the internals instead of
  [`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md).
- Avoid [`gsub()`](https://rdrr.io/r/base/grep.html) in `store_init()`.
- Avoid repeated calls to `meta$get_record()` in `builder_should_run()`.
- Mock the store object when creating a record from a metadata row.
- Avoid
  [`cli::col_none()`](https://cli.r-lib.org/reference/ansi-styles.html)
  to reduce the number of ANSI characters printed to the R console.

## targets 1.0.0

CRAN release: 2023-04-24

`targets` is moving to version 1.0.0 because it is significantly more
mature than previous versions. Specifically,

1.  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
    now integrates with `crew`, which will significantly improve the way
    `targets` does high-performance computing going forward.
2.  All other functionality in `targets` has stabilized. There is still
    room for smaller new features, but none as large as `crew`
    integration, none that will fundamentally change how the package
    operates.

### Major improvements

- Support distributed computing through the `crew` package in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  ([\#753](https://github.com/ropensci/targets/issues/753)). `crew`
  itself is still in its early stages and currently lacks the launcher
  plugins to match the `clustermq` and `future` backends, but long-term,
  `crew` will be the predominant high-performance computing backend.

### Minor improvements

- Add a new `store_copy_object()` to the store class to enable
  `"fst_dt"` and other formats to make deep copies when needed
  ([\#1041](https://github.com/ropensci/targets/issues/1041),
  [@MilesMcBain](https://github.com/MilesMcBain)).
- Add a new `copy` argument to allow
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  formats to set the `store_copy_object()` method
  ([\#1041](https://github.com/ropensci/targets/issues/1041),
  [@MilesMcBain](https://github.com/MilesMcBain)).
- Shorten the output string returned by
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  when default methods are used.
- Add a `change_directory` argument to
  [`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md)
  ([\#1040](https://github.com/ropensci/targets/issues/1040),
  [@dipterix](https://github.com/dipterix)).
- In `format = "url"` targets, implement retries and timeouts when
  connecting to URLs. The default timeout is 10 seconds, and the default
  retry interval is 1 second. Both are configurable via
  [`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md)
  ([\#1048](https://github.com/ropensci/targets/issues/1048)).
- Use
  [`parallelly::freePort()`](https://parallelly.futureverse.org/reference/freePort.html)
  in
  [`tar_random_port()`](https://docs.ropensci.org/targets/reference/tar_random_port.md).
- Rename a target and a function in the
  [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)
  example pipeline
  ([\#1033](https://github.com/ropensci/targets/issues/1033),
  [@b-rodrigues](https://github.com/b-rodrigues)).
- Edit the description.

## targets 0.14.3

CRAN release: 2023-03-08

- Handle encoding errors while trying to process error and warning
  messages ([\#1019](https://github.com/ropensci/targets/issues/1019),
  [@adrian-quintario](https://github.com/adrian-quintario)).
- Fix S3 generic/method consistency.

## targets 0.14.2

CRAN release: 2023-01-06

- Forward user-level custom error conditions to the top of the pipeline
  ([\#997](https://github.com/ropensci/targets/issues/997),
  [@alexverse](https://github.com/alexverse)).
- Link to the help page of the manual.

## targets 0.14.1

CRAN release: 2022-11-29

- Fix the command inserted for debug mode
  ([\#975](https://github.com/ropensci/targets/issues/975)).
- Set empty chunk options to ensure Target Markdown compatibility with
  the special “setup” chunk
  ([\#973](https://github.com/ropensci/targets/issues/973),
  [@KaiAragaki](https://github.com/KaiAragaki)).
- Only store the first 50 warnings in the metadata, and cap the text of
  the warning messages at 2048 characters
  ([\#983](https://github.com/ropensci/targets/issues/983),
  [@thejokenott](https://github.com/thejokenott)).
- Enhance the
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
  help file ([\#988](https://github.com/ropensci/targets/issues/988),
  [@Sage0614](https://github.com/Sage0614)).
- Implement `destroy = "user"` in
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md).

## targets 0.14.0

CRAN release: 2022-11-01

- Move `#!/bin/sh` line to the top of SLURM `clustermq` template file
  ([\#944](https://github.com/ropensci/targets/issues/944),
  [\#955](https://github.com/ropensci/targets/issues/955),
  [@GiuseppeTT](https://github.com/GiuseppeTT)).
- Add new function
  [`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md).
- Rename
  [`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md)
  to
  [`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md)
  with deprecation.
- Rename
  [`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md)
  to
  [`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md)
  with deprecation.
- Add new function
  [`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md).
- Make Target Markdown target scripts dynamically locate their support
  scripts so the appropriate scripts can be found even when they are
  generated from one directory and sourced from another
  ([\#953](https://github.com/ropensci/targets/issues/953),
  [\#957](https://github.com/ropensci/targets/issues/957),
  [@TylerGrantSmith](https://github.com/TylerGrantSmith)).
- Allow user-side control of the seeds at the pipeline level.
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  now supports a `seed` argument, and target-specific seeds are
  determined by `tar_option_get("seed")` and the target name.
  `tar_option_set(seed = NA)` disables seed-setting behavior but
  forcibly invalidates all the affected targets except when `seed` is
  `FALSE` in the target’s
  [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
  ([\#882](https://github.com/ropensci/targets/issues/882),
  [@sworland-thyme](https://github.com/sworland-thyme),
  [@joelnitta](https://github.com/joelnitta)).
- Implement a `seed` argument in
  [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
  to control whether targets update in response to changing or `NA`
  seeds ([\#882](https://github.com/ropensci/targets/issues/882),
  [@sworland-thyme](https://github.com/sworland-thyme),
  [@joelnitta](https://github.com/joelnitta)).
- Reduce the number of per-target AWS/GCP storage API calls. Previously
  there were 3 API calls per target, including 2 HEAD requests. Now
  there is just 1 for a typical target (unless dependencies have to be
  downloaded). Relies on S3 strong read-after-write consistency
  ([\#958](https://github.com/ropensci/targets/issues/958)).
- Update the
  [`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md)
  workflow file to use `@v2`
  ([\#960](https://github.com/ropensci/targets/issues/960),
  [@kulinar](https://github.com/kulinar)).
- Print helpful hints while debugging a target interactively
  ([\#961](https://github.com/ropensci/targets/issues/961)).
- Only attempt to debug a target when `callr_function` is `NULL`
  ([\#961](https://github.com/ropensci/targets/issues/961)).
- Make formats `"feather"`, `"parquet"`, `"file"`, and `"url"` work with
  `error = "null"`
  ([\#969](https://github.com/ropensci/targets/issues/969)).
- Declare formats `"keras"` and `"torch"` superseded by
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md).
  Documented in the
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  help file.
- Declare formats `"keras"` and `"torch"` incompatible with
  `error = "null"`. Documented in the
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  help file and in a warning thrown by
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  via
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md).
- Add a `convert` argument to
  [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  to allow custom `store_convert_object()` methods
  ([\#970](https://github.com/ropensci/targets/issues/970)).

## targets 0.13.5

CRAN release: 2022-09-26

- Use [`any_of()`](https://tidyselect.r-lib.org/reference/all_of.html)
  instead of
  [`all_of()`](https://tidyselect.r-lib.org/reference/all_of.html) in
  tests to ensure compatibility with `tidyselect` 1.1.2.9000
  ([\#928](https://github.com/ropensci/targets/issues/928),
  [@hadley](https://github.com/hadley)).
- Make the `run.R` from
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  executable ([\#929](https://github.com/ropensci/targets/issues/929),
  [@petrbouchal](https://github.com/petrbouchal)).
- Add `#!/usr/bin/env Rscript` to the top of `run.R` from
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  ([\#929](https://github.com/ropensci/targets/issues/929),
  [@petrbouchal](https://github.com/petrbouchal)).

## targets 0.13.4

CRAN release: 2022-09-15

- Implement custom alternative to `skip_on_cran()` to avoid
  <https://github.com/r-lib/testthat/issues/1470#issuecomment-1248145555>.
- Skip more tests on CRAN.

## targets 0.13.3

### Enhancements

- Print “no targets found” when there are no targets in the pipeline to
  check or build, or if the `names` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  does not identify any such targets in the pipeline
  ([\#923](https://github.com/ropensci/targets/issues/923),
  [@llrs](https://github.com/llrs)).
- Ignore `.packageName`, `.__NAMESPACE__.`, and `.__S3MethodsTable__.`
  when importing objects from packages with the `imports` option of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).
- Import datasets from packages in the `imports` option of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  ([\#926](https://github.com/ropensci/targets/issues/926),
  [@joelnitta](https://github.com/joelnitta)).
- Print target-specific elapsed runtimes in the verbose and timestamp
  reporters.
- Improve error messages in functions like
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  and
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  when the data store is missing.

## targets 0.13.2

### Bug fixes

- Do not incorrectly reference feather resources for parquet storage.

### Enhancements

- Simplify and improve error handling.
- In the `command` column of
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
  output, separate lines with “” instead of “\n” so the text output is
  straightforward to work with.
- Add a `drop_missing` argument to
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
  to hide/show columns with all `NA` values.
- Do not set Parquet version.

## targets 0.13.1

CRAN release: 2022-08-05

- Fix reverse dependency checks.

## targets 0.13.0

### Bug fixes

- Do not bootstrap the junction of a stem unless the target is branched
  over ([\#858](https://github.com/ropensci/targets/issues/858),
  [@dipterix](https://github.com/dipterix)).
- For non-“file” AWS targets, immediately delete the scratch file after
  the target is uploaded
  ([\#889](https://github.com/ropensci/targets/issues/889),
  [@stuvet](https://github.com/stuvet)).

### New features

- Allow extra arguments to `paws` functions via `...` in
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  ([\#855](https://github.com/ropensci/targets/issues/855),
  [@michkam89](https://github.com/michkam89)).
- Add
  [`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md)
  to conveniently source R scripts (e.g. in `_targets.R`).

### Enhancements

- Color ordinary `targets` messages the default theme color, and color
  warnings and errors red
  ([\#856](https://github.com/ropensci/targets/issues/856),
  [@gorkang](https://github.com/gorkang)).
- Automatically supply job names in the scripts generated by
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md).
- Inherit resources one-by-one in nested fashion from
  `tar_option_get("resources")`
  ([\#892](https://github.com/ropensci/targets/issues/892)). See the
  revised `"Resources"` section of the
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  help file for details.

## targets 0.12.1

CRAN release: 2022-06-03

### New features

- Add arguments `legend` and `color` to further configure
  [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)
  ([\#848](https://github.com/ropensci/targets/issues/848),
  [@noamross](https://github.com/noamross)).
- For HPC schedulers like SLURM and SGE,
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  now creates a `job.sh` script to run the pipeline as a cluster job
  ([\#839](https://github.com/ropensci/targets/issues/839)).

### Enhancements

- Use lapply() to source scripts in
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md).
  Avoids defining a global variable for the file.
- Recursively find scripts to source in the
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  `_targets.R` file.
- Refactor error printing.

## targets 0.12.0

CRAN release: 2022-04-19

### Bug fixes

- Fix
  [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)
  graph ordering.
- Hash the node names and quote the label names of
  [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)
  graphs to avoid JavaScript keywords.
- Remove superfluous line breaks in the node labels of graph visuals.
- Fix metadata migration to version \>= 0.10.0
  ([\#812](https://github.com/ropensci/targets/issues/812),
  [@tjmahr](https://github.com/tjmahr)).
- [`data.table::fread()`](https://rdrr.io/pkg/data.table/man/fread.html)
  with encoding equal to `getOption("encoding")` if available
  ([\#814](https://github.com/ropensci/targets/issues/814),
  [@svraka](https://github.com/svraka)). Only works with UTF-8 and
  latin1 because that is what `data.table` supports.
- Force add files in GitHub Actions workflow job
  ([\#815](https://github.com/ropensci/targets/issues/815),
  [@tarensanders](https://github.com/tarensanders)).

### New features

- [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  now writes a `_targets.R` file tailored to the project in the current
  working directory
  ([\#639](https://github.com/ropensci/targets/issues/639),
  [@noamross](https://github.com/noamross)).
- Move the old
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  to
  [`use_targets_rmd()`](https://docs.ropensci.org/targets/reference/use_targets_rmd.md).

### Enhancements

- Load packages when loading data for downstream targets in the pipeline
  ([\#713](https://github.com/ropensci/targets/issues/713)).
- Handle edge case when `getOption("OutDec")` is not `"."` to prevent
  time stamps from being corrupted
  ([\#433](https://github.com/ropensci/targets/issues/433),
  [@jarauh](https://github.com/jarauh)).
- Added helper function
  [`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md)
  to quickly load all targets
  ([\#823](https://github.com/ropensci/targets/issues/823),
  [@malcolmbarrett](https://github.com/malcolmbarrett))

## targets 0.11.0

CRAN release: 2022-03-18

### Bug fixes

- Print out the relevant target names if targets have conflicting names.
- Catch all the target warnings instead of just reporting the last one.
- Allow 200 group URL status codes instead of just 200
  ([\#797](https://github.com/ropensci/targets/issues/797),
  [@petrbouchal](https://github.com/petrbouchal)).

### New features

- Add Google Cloud Storage via `tar_target(..., repository = "gcp")`
  ([\#720](https://github.com/ropensci/targets/issues/720),
  [@markedmondson1234](https://github.com/markedmondson1234)). Special
  thanks to [@markedmondson1234](https://github.com/markedmondson1234)
  for the cloud storage utilities in `R/utils_gcp.R`
- `mermaid.js` static graphs with
  [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)
  ([\#775](https://github.com/ropensci/targets/issues/775),
  [@yonicd](https://github.com/yonicd)).
- Implement `tar_target(..., error = "null")`to allow errored targets to
  return `NULL` and continue
  ([\#807](https://github.com/ropensci/targets/issues/807),
  [@zoews](https://github.com/zoews)). Errors are still registered,
  those targets are not up to date, and downstream targets have an
  easier time continuing on.
- Implement
  [`tar_assert_finite()`](https://docs.ropensci.org/targets/reference/tar_assert.md).
- [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md),
  [`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md),
  and
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
  now attempt to delete cloud data for the appropriate targets
  ([\#799](https://github.com/ropensci/targets/issues/799)). In
  addition,
  [`tar_exist_objects()`](https://docs.ropensci.org/targets/reference/tar_exist_objects.md)
  and
  [`tar_objects()`](https://docs.ropensci.org/targets/reference/tar_objects.md)
  now report about target data in the cloud when applicable. Add a new
  `cloud` argument to each function to optionally suppress this new
  behavior.
- Add a `zoom_speed` argument to
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  and
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  ([\#749](https://github.com/ropensci/targets/issues/749),
  [@dipterix](https://github.com/dipterix)).
- Report the total runtime of the pipeline in the `"verbose"`,
  `"verbose_positives"`, `"timestamp"`, and `"timesamp_positives"`
  reporters.

### Enhancements

- Allow target name character strings to have attributes
  ([\#758](https://github.com/ropensci/targets/issues/758),
  [@psanker](https://github.com/psanker)).
- Sort metadata rows when the pipeline finishes so that
  version-controlling the metadata is easier
  ([\#766](https://github.com/ropensci/targets/issues/766),
  [@jameelalsalam](https://github.com/jameelalsalam)).

### Deprecations

- Deprecate the `"aws_*"` storage format values in favor of a new
  `repository` argument
  ([\#803](https://github.com/ropensci/targets/issues/803)). In other
  words, `tar_target(..., format = "aws_qs")` is now
  `tar_target(..., format = "qs", repository = "aws")`. And internally,
  storage classes with multiple inheritance are created dynamically as
  opposed to having hard-coded source files. All this paves the way to
  add new cloud storage platforms without combinatorial chaos.

## targets 0.10.0

CRAN release: 2022-01-07

### Bug fixes

- Add class `"tar_nonexportable"` to `format = "aws_keras"` and
  `format = "aws_torch"` stores.
- Export S3 methods of generic `tar_make_interactive_load_target()`.

### New features

- Allow entirely custom storage formats through
  `tar_target(format = tar_format(...))`
  ([\#736](https://github.com/ropensci/targets/issues/736)).
- Add a new function
  [`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md)
  to return the `targets` function currently running (from `_targets.R`
  or a target).
- Add a new function
  [`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md)
  to tell whether the pipeline is currently running. Detects if it is
  called from
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  or similar function.

### Enhancements

- Add `Sys.getenv("TAR_PROJECT")` to the output of
  [`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md).
- Set the `store` field of `tar_runtime` prior to sourcing `_targets.R`
  so
  [`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md)
  works in target scripts.
- Explicitly export all the environment variables from
  [`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md)
  to targets run on parallel workers.
- Allow `format = "file"` targets to return `character(0)`
  ([\#728](https://github.com/ropensci/targets/issues/728),
  [@programLyrique](https://github.com/programLyrique)).
- Automatically remove non-targets from the target list and improve
  target list error messages
  ([\#731](https://github.com/ropensci/targets/issues/731),
  [@billdenney](https://github.com/billdenney)).
- Link to resources on deploying to RStudio Connect
  ([\#745](https://github.com/ropensci/targets/issues/745),
  [@ian-flores](https://github.com/ian-flores)).

## targets 0.9.1

- Mask pointers in function dependencies
  ([\#721](https://github.com/ropensci/targets/issues/721),
  [@matthiaskaeding](https://github.com/matthiaskaeding))

## targets 0.9.0

CRAN release: 2021-12-04

### Highlights

- Track the version ID of AWS S3-backed targets if the bucket is
  version-enabled
  ([\#711](https://github.com/ropensci/targets/issues/711)). If you put
  your targets in AWS and the metadata and code under version control,
  you can `git checkout` a different branch of your code and all you
  targets will stay up to date.
- Refactor the AWS path format internally. It now consists of
  arbitrarily extensible key-value pairs so more AWS S3 functionality
  may be added more seamlessly going forward
  ([\#711](https://github.com/ropensci/targets/issues/711)).
- Switch the AWS S3 backend to `paws`
  ([\#711](https://github.com/ropensci/targets/issues/711)).

### New features

- Add a `region` argument to
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  to allow the user to explicitly declare a region for each AWS S3
  buckets ([@caewok](https://github.com/caewok),
  [\#681](https://github.com/ropensci/targets/issues/681)). Different
  buckets can now have different regions. This feature required
  modifying the metadata path for AWS storage formats. Before, the first
  element of the path was simply the bucket name. Now, it is internally
  formatted like `"bucket=BUCKET:region=REGION"`, where `BUCKET` is the
  user-supplied bucket name and `REGION` is the user-supplied region
  name. The new `targets` is back-compatible with the old metadata
  format, but if you run the pipeline with `targets` \>= 0.8.1.9000 and
  then downgrade to `targets` \<= 0.8.1, any AWS targets will break.
- Add new reporters `timestamp_positives"` and `"verbose_positives"`
  that omit messages for skipped targets
  ([@psanker](https://github.com/psanker),
  [\#683](https://github.com/ropensci/targets/issues/683)).
- Implement
  [`tar_assert_file()`](https://docs.ropensci.org/targets/reference/tar_assert.md).
- Implement
  [`tar_reprex()`](https://docs.ropensci.org/targets/reference/tar_reprex.md)
  for creating easier reproducible examples of pipelines.
- Implement
  [`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md)
  to get the path to the store of the currently running pipeline
  ([\#714](https://github.com/ropensci/targets/issues/714),
  [@MilesMcBain](https://github.com/MilesMcBain)).
- Automatically write a `_targets/user/` folder to encourage
  `gittargets` users to put custom files there for data version control.

### Bug fixes

- Make sure
  [`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md)
  uses the current store path of the currently running pipeline instead
  of `tar_config_get("store")`
  ([\#714](https://github.com/ropensci/targets/issues/714),
  [@MilesMcBain](https://github.com/MilesMcBain)).

### Enhancements

- Refactor the automatic `.gitignore` file inside the data store to
  allow the metadata to be committed to version control more easily
  ([\#685](https://github.com/ropensci/targets/issues/685),
  [\#711](https://github.com/ropensci/targets/issues/711)).
- Document target name requirements in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  and
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  ([@tjmahr](https://github.com/tjmahr),
  [\#679](https://github.com/ropensci/targets/issues/679)).
- Catch and relay any the error if a target cannot be checked in
  `target_should_run.tar_builder()`. These kinds of errors sometimes
  come up with AWS storage.
- Fix the documentation of the reporters.
- Only write `_targets/.gitignore` for new data stores so the user can
  delete the `.gitignore` file without it mysteriously reappearing
  ([\#685](https://github.com/ropensci/targets/issues/685)).

## targets 0.8.1

CRAN release: 2021-10-26

### New features

- Add arguments `strict` and `silent` to allow
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  and
  [`tar_load_raw()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  to bypass targets that cannot be loaded.

### Enhancements

- Improve `tidyselect` docs in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  ([\#640](https://github.com/ropensci/targets/issues/640),
  [@dewoller](https://github.com/dewoller)).
- Use namespaced call to
  [`tar_dir()`](https://docs.ropensci.org/targets/reference/tar_dir.md)
  in
  [`tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.md)
  ([\#642](https://github.com/ropensci/targets/issues/642),
  [@billdenney](https://github.com/billdenney)).
- Improve
  [`tar_assert_target_list()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  error message ([@kkami1115](https://github.com/kkami1115),
  [\#654](https://github.com/ropensci/targets/issues/654)).
- Throw an informative error if a target name starts with a dot
  ([@dipterix](https://github.com/dipterix),
  [\#662](https://github.com/ropensci/targets/issues/662)).
- Improve help files of
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
  and related cleanup functions
  ([@billdenney](https://github.com/billdenney),
  [\#675](https://github.com/ropensci/targets/issues/675)).

## targets 0.8.0

CRAN release: 2021-09-21

### Bug fixes

- Hash the correct files in
  `tar_target(target_name, ..., format = "aws_file")`. Previously,
  `_targets/objects/target_name` was also hashed if it existed.

### New features

- Implement a new
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  function to delete one or more configuration settings from the YAML
  configuration file.
- Implement the `TAR_CONFIG` environment variable to set the default
  file path of the YAML configuration file with project settings
  ([\#622](https://github.com/ropensci/targets/issues/622),
  [@yyzeng](https://github.com/yyzeng),
  [@atusy](https://github.com/atusy),
  [@nsheff](https://github.com/nsheff),
  [@wdkrnls](https://github.com/wdkrnls)). If `TAR_CONFIG` is not set,
  the file path is still `_targets.yaml`.
- Restructure the YAML configuration file format to handle configuration
  information for multiple projects (using the `config` package) and
  support the `TAR_PROJECT` environment variable to select the current
  active project for a given R session. The old single-project format is
  gracefully deprecated
  ([\#622](https://github.com/ropensci/targets/issues/622),
  [@yyzeng](https://github.com/yyzeng),
  [@atusy](https://github.com/atusy),
  [@nsheff](https://github.com/nsheff),
  [@wdkrnls](https://github.com/wdkrnls)).
- Implement `retrieval = "none"` and `storage = "none"` to anticipate
  loading/saving targets from other languages, e.g. Julia
  ([@MilesMcBain](https://github.com/MilesMcBain)).
- Add a new
  [`tar_definition()`](https://docs.ropensci.org/targets/reference/tar_definition.md)
  function to get the target definition object of the current target
  while that target is running in a pipeline.
- If called inside an AWS target,
  [`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md)
  now returns the path to the staging file instead of
  `_targets/objects/target_name`. This ensures you can still write to
  [`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md)
  in `storage = "none"` targets and the package will automatically hash
  the right file and upload it to the cloud. (This behavior does not
  apply to formats `"file"` and `"aws_file"`, where it is never
  necessary to set `storage = "none"`.)

### Enhancements

- Use `eval(parse(text = ...), envir = tar_option_set("envir")` instead
  of [`source()`](https://rdrr.io/r/base/source.html) in the
  `_targets.R` file for Target Markdown.
- Allow feather and parquet formats to accept objects of class
  `RecordBatch` and `Table`
  ([@MilesMcBain](https://github.com/MilesMcBain)).
- Let `knitr` load the Target Markdown engine
  ([\#469](https://github.com/ropensci/targets/issues/469),
  [@nviets](https://github.com/nviets),
  [@yihui](https://github.com/yihui)). Minimum `knitr` version is now
  `1.34`.
- In the
  [`tar_resources_future()`](https://docs.ropensci.org/targets/reference/tar_resources_future.md)
  help file, encourage the use of `plan` to specify resources.

## targets 0.7.0

CRAN release: 2021-08-19

### Bug fixes

- Ensure `error = "continue"` does not cause errored targets to have
  `NULL` values.
- Relay output and messages in Target Markdown interactive mode (using
  the R/default `knitr` engine).

### New features

- Expose the `poll_connection`, `stdout`, and `stderr` arguments of
  [`callr::r_bg()`](https://callr.r-lib.org/reference/r_bg.html) in
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  ([@mpadge](https://github.com/mpadge)).
- Add new helper functions to list targets in each progress category:
  [`tar_started()`](https://docs.ropensci.org/targets/reference/tar_started.md),
  [`tar_skipped()`](https://docs.ropensci.org/targets/reference/tar_skipped.md),
  [`tar_built()`](https://docs.ropensci.org/targets/reference/tar_built.md),
  [`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md),
  and
  [`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md).
- Add new helper functions
  [`tar_interactive()`](https://docs.ropensci.org/targets/reference/tar_interactive.md),
  [`tar_noninteractive()`](https://docs.ropensci.org/targets/reference/tar_noninteractive.md),
  and
  [`tar_toggle()`](https://docs.ropensci.org/targets/reference/tar_toggle.md)
  to differentially suppress code in non-interactive and interactive
  mode in Target Markdown
  ([\#607](https://github.com/ropensci/targets/issues/607),
  [@33Vito](https://github.com/33Vito)).

### Enhancements

- Handle `future` errors within targets
  ([\#570](https://github.com/ropensci/targets/issues/570),
  [@stuvet](https://github.com/stuvet)).
- Handle storage errors within targets
  ([\#571](https://github.com/ropensci/targets/issues/571),
  [@stuvet](https://github.com/stuvet)).
- In Target Markdown in non-interactive mode, suppress messages if the
  `message` `knitr` chunk option is `FALSE`
  ([\#574](https://github.com/ropensci/targets/issues/574),
  [@jmbuhr](https://github.com/jmbuhr)).
- In Target Markdown, if `tar_interactive` is not set, choose
  interactive vs non-interactive mode based on
  `isTRUE(getOption("knitr.in.progress"))` instead of
  [`interactive()`](https://rdrr.io/r/base/interactive.html).
- Convert errors loading dependencies into errors running targets
  ([@stuvet](https://github.com/stuvet)).

## targets 0.6.0

CRAN release: 2021-07-21

### Bug fixes

- Allow
  [`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md)
  to lose and then regain connection to the progress file.
- Make sure changes to the `tar_group` column of `iteration = "group"`
  data frames do not invalidate slices
  ([\#507](https://github.com/ropensci/targets/issues/507),
  [@lindsayplatt](https://github.com/lindsayplatt)).

### New features

- In Target Markdown, add a new `tar_interactive` global option to
  select interactive mode or non-interactive mode
  ([\#469](https://github.com/ropensci/targets/issues/469)).
- Highlight a graph neighborhood when the user clicks a node. Control
  the neighborhood degree with new arguments `degree_from` and
  `degree_to` of
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  and
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  ([\#474](https://github.com/ropensci/targets/issues/474),
  [@rgayler](https://github.com/rgayler)).
- Make the target script path configurable in
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  ([\#476](https://github.com/ropensci/targets/issues/476)).
- Add a `tar_script` chunk option in Target Markdown to control where
  the [targets](https://docs.ropensci.org/targets/) language engine
  writes the target script and helper scripts
  ([\#478](https://github.com/ropensci/targets/issues/478)).
- Add new arguments `script` and `store` to choose custom paths to the
  target script file and data store for individual function calls
  ([\#477](https://github.com/ropensci/targets/issues/477)).
- Allow users to set an alternative path to the YAML configuration file
  for the current R session
  ([\#477](https://github.com/ropensci/targets/issues/477)). Most users
  have no reason to set this path, it is only for niche applications
  like Shiny apps with `targets` backends. Unavoidably, the path gets
  reset to `_targets.yaml` when the session restarts.
- Add new `_targets.yaml` config options `reporter_make`,
  `reporter_outdated`, and `workers` to control function argument
  defaults shared across multiple functions called outside `_targets.R`
  ([\#498](https://github.com/ropensci/targets/issues/498),
  [@ianeveperry](https://github.com/ianeveperry)).
- Add
  [`tar_load_globals()`](https://docs.ropensci.org/targets/reference/tar_load_globals.md)
  for debugging, testing, prototyping, and teaching
  ([\#496](https://github.com/ropensci/targets/issues/496),
  [@malcolmbarrett](https://github.com/malcolmbarrett)).
- Add structure to the `resources` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  to avoid conflicts among formats and HPC backends
  ([\#489](https://github.com/ropensci/targets/issues/489)). Includes
  user-side helper functions like
  [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  and
  [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  to build the required data structures.
- Log skipped targets in `_targets/meta/progress` and display then in
  [`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md),
  [`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md),
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
  [`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md),
  [`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md),
  and
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  ([\#514](https://github.com/ropensci/targets/issues/514)). Instead of
  writing each skip line separately to `_targets/meta/progress`,
  accumulate skip lines in a queue and then write them all out in bulk
  when something interesting happens. This avoids a lot of overhead in
  certain cases.
- Add a `shortcut` argument to
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md),
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md),
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md),
  and
  [`tar_sitrep()`](https://docs.ropensci.org/targets/reference/tar_sitrep.md)
  to more efficiently skip parts of the pipeline
  ([\#522](https://github.com/ropensci/targets/issues/522),
  [\#523](https://github.com/ropensci/targets/issues/523),
  [@jennysjaarda](https://github.com/jennysjaarda),
  [@MilesMcBain](https://github.com/MilesMcBain),
  [@kendonB](https://github.com/kendonB)).
- Support `names` and `shortcut` in graph data frames and graph visuals
  ([\#529](https://github.com/ropensci/targets/issues/529)).
- Move `allow` and `exclude` to the network behind the graph visuals
  rather than the visuals themselves
  ([\#529](https://github.com/ropensci/targets/issues/529)).
- Add a new “progress” display to the
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  app to show verbose progress info and metadata.
- Add a new `workspace_on_error` argument of
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  to supersede `error = "workspace"`. Helps control workspace behavior
  independently of the `error` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  ([\#405](https://github.com/ropensci/targets/issues/405),
  [\#533](https://github.com/ropensci/targets/issues/533),
  [\#534](https://github.com/ropensci/targets/issues/534),
  [@mattwarkentin](https://github.com/mattwarkentin),
  [@xinstein](https://github.com/xinstein)).
- Implement `error = "abridge"` in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  and related functions. If a target errors out with this option, the
  target itself stops, any currently running targets keeps, and no new
  targets launch after that
  ([\#533](https://github.com/ropensci/targets/issues/533),
  [\#534](https://github.com/ropensci/targets/issues/534),
  [@xinstein](https://github.com/xinstein)).
- Add a menu prompt to
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
  which can be suppressed with `TAR_ASK = "false"`
  ([\#542](https://github.com/ropensci/targets/issues/542),
  [@gofford](https://github.com/gofford)).
- Support functions
  [`tar_older()`](https://docs.ropensci.org/targets/reference/tar_older.md)
  and
  [`tar_newer()`](https://docs.ropensci.org/targets/reference/tar_newer.md)
  to help users identify and invalidate targets at regular times or
  intervals.

### Deprecations

- In Target Markdown, deprecate the `targets` chunk option in favor of
  `tar_globals`
  ([\#469](https://github.com/ropensci/targets/issues/469)).
- Deprecate `error = "workspace"` in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  and related functions. Use `tar_option_set(workspace_on_error = TRUE)`
  instead ([\#405](https://github.com/ropensci/targets/issues/405),
  [\#533](https://github.com/ropensci/targets/issues/533),
  [@mattwarkentin](https://github.com/mattwarkentin),
  [@xinstein](https://github.com/xinstein)).

### Performance

- Reset the backoff upper bound when concluding a target or shutting
  down a `clustermq` worker
  ([@rich-payne](https://github.com/rich-payne)).
- Set more aggressive default backoff bound of 0.1 seconds (previous: 5
  seconds) and set a more aggressive minimum of 0.001 seconds (previous:
  0.01 seconds) ([@rich-payne](https://github.com/rich-payne)).
- Speed up the summary and forecast reporters by only printing to the
  console every quarter second.
- Avoid superfluous calls to `store_sync_file_meta.default()` on small
  files.
- In
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
  take several measures to avoid long computation times rendering the
  graph:
  - Expose arguments `display` and `displays` to
    [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
    so the user can select which display shows first.
  - Make `"summary"` the default display instead of `"graph"`.
  - Set `outdated` to `FALSE` by default.

### Enhancements

- Simplify the Target Markdown example.
- Warn about unnamed chunks in Target Markdown.
- Redesign option system to be more object-oriented and rigorous. Also
  export most options to HPC workers
  ([\#475](https://github.com/ropensci/targets/issues/475)).
- Simplify config system to let API function arguments take control
  ([\#483](https://github.com/ropensci/targets/issues/483)).
- In
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  for targets with `format = "aws_file"`, download the file back to the
  path the user originally saved it when the target ran.
- Replace the `TAR_MAKE_REPORTER` environment variable with
  `targets::tar_config_get("reporter_make")`.
- Use `eval(parse(text = readLines("_targets.R")), envir = some_envir)`
  and related techniques instead of the less controllable
  [`source()`](https://rdrr.io/r/base/source.html). Expose an `envir`
  argument to many functions for further control over evaluation if
  `callr_function` is `NULL`.
- Drop `out.attrs` when hashing groups of data frames to extend
  [\#507](https://github.com/ropensci/targets/issues/507) to
  [`expand.grid()`](https://rdrr.io/r/base/expand.grid.html)
  ([\#508](https://github.com/ropensci/targets/issues/508)).
- Increase the number of characters in errors and warnings up to 2048.
- Refactor assertions to automatically generate better messages.
- Export assertions, conditions, and language utilities in packages that
  build on top of `targets`.
- Change `GITHUBPAT` to `GITHUB_TOKEN` in the
  [`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md)
  YAML file ([\#554](https://github.com/ropensci/targets/issues/554),
  [@eveyp](https://github.com/eveyp)).
- Support the `eval` chunk option in Target Markdown
  ([\#552](https://github.com/ropensci/targets/issues/552),
  [@fkohrt](https://github.com/fkohrt)).
- Record time stamps in the metadata `time` column for all builder
  targets, regardless of storage format.

## targets 0.5.0

### Bug fixes

- Export in-memory config settings from `_targets.yaml` to parallel
  workers.

### New features

- Add a limited-scope `exclude` argument to
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  and
  [`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md)
  ([\#458](https://github.com/ropensci/targets/issues/458),
  [@gorkang](https://github.com/gorkang)).
- Write a `.gitignore` file to ignore everything in `_targets/meta/`
  except `.gitignore` and `_targets/meta/meta`.
- Target Markdown: add `knitr` engines for pipeline construction and
  prototyping from within literate programming documents
  ([\#469](https://github.com/ropensci/targets/issues/469),
  [@cderv](https://github.com/cderv),
  [@nviets](https://github.com/nviets),
  [@emilyriederer](https://github.com/emilyriederer),
  [@ijlyttle](https://github.com/ijlyttle),
  [@GShotwell](https://github.com/GShotwell),
  [@gadenbuie](https://github.com/gadenbuie),
  [@tomsing1](https://github.com/tomsing1)). Huge thanks to
  [@cderv](https://github.com/cderv) on this one for answering my deluge
  of questions, helping me figure out what was and was not possible in
  `knitr`, and ultimately circling me back to a successful approach.
- Add an RStudio R Markdown template for Target Markdown
  ([\#469](https://github.com/ropensci/targets/issues/469)).
- Implement
  [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md),
  which writes the Target Markdown template to the project root
  ([\#469](https://github.com/ropensci/targets/issues/469)).
- Implement
  [`tar_unscript()`](https://docs.ropensci.org/targets/reference/tar_unscript.md)
  to clean up scripts written by Target Markdown.

### Enhancements

- Enable priorities in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  and
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md).
- Show the priority in the print method of stem and pattern targets.
- Throw informative errors if the secondary arguments to
  `pattern = slice()` or `pattern = sample()` are invalid.
- In
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md),
  assert that commands have length 1 when converted to expressions.
- Handle errors and post failure artifacts in the Github Actions YAML
  file.
- Rewrite the documentation on invalidation rules in
  [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
  ([@maelle](https://github.com/maelle)).
- Drop `dplyr` groups and `"grouped_df"` class in
  [`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.md)
  (`tarchetypes` discussion
  [\#53](https://github.com/ropensci/targets/issues/53),
  [@kendonB](https://github.com/kendonB)).
- Assign branch names to dynamic branching return values produced by
  [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  and
  [`tar_read_raw()`](https://docs.ropensci.org/targets/reference/tar_read.md).

## targets 0.4.2

CRAN release: 2021-04-30

### Bug fixes

- Do not use time stamps to monitor the config file
  (e.g. `_targets.yaml`). Fixes CRAN check errors from version 0.4.1.

## targets 0.4.1

CRAN release: 2021-04-22

- Fix CRAN test error on Windows R-devel.
- Do not inherit `roxygen2` docstrings from `shiny`.
- Handle more missing `Suggests:` packages.
- Unset the config lock before reading `targets.yaml` in the `callr`
  process.

## targets 0.4.0

### Bug fixes

- Avoid [`file.rename()`](https://rdrr.io/r/base/files.html) errors when
  migrating staged temporary files
  ([\#410](https://github.com/ropensci/targets/issues/410)).
- Return correct error messages from feather and parquet formats
  ([\#388](https://github.com/ropensci/targets/issues/388)). Now calling
  `assert_df()` from `store_assert_format()` instead of
  `store_cast_object()`. And now those last two functions are not called
  at all if the target throws an error.
- Retry writing lines to database files so Windows machines can run
  [`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md)
  at the same time as the pipeline
  ([\#393](https://github.com/ropensci/targets/issues/393)).
- Rename file written by
  [`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md)
  to `_targets_packages.R`
  ([\#397](https://github.com/ropensci/targets/issues/397)).
- Ensure metadata is loaded to compute labels properly when
  `outdated = FALSE` in
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md).

### New features

- Implement
  [`tar_timestamp()`](https://docs.ropensci.org/targets/reference/tar_timestamp.md)
  and
  [`tar_timestamp_raw()`](https://docs.ropensci.org/targets/reference/tar_timestamp.md)
  to get the last modified timestamp of a target’s data
  ([\#378](https://github.com/ropensci/targets/issues/378)).
- Implement
  [`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md)
  to compactly summarize all pipeline progress
  ([\#380](https://github.com/ropensci/targets/issues/380)).
- Add a `characters` argument of
  [`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md)
  to cap the traceback line lengths
  ([\#383](https://github.com/ropensci/targets/issues/383)).
- Add new “summary” and “about” views to
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  ([\#382](https://github.com/ropensci/targets/issues/382)).
- Implement
  [`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md)
  to repeatedly poll runtime progress in the R console
  ([\#381](https://github.com/ropensci/targets/issues/381)).
  [`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md)
  is a lightweight alternative to
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md).
- Change the color of the “dormant” status in the graph.
- Add a `tar_envvar()` function to list values of special environment
  variables supported in `targets`. The help file explains each
  environment variable in detail.
- Support extra project-level configuration settings with
  `_targets.yaml`
  ([\#297](https://github.com/ropensci/targets/issues/297)). New
  functions
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  interact with the `_targets.yaml` file. Currently only supports the
  `store` field to set the data store path to something other than
  `_targets/`.

### Performance

- Shut down superfluous persistent workers earlier in dynamic branching
  and when all remaining targets have `deployment = "main"`
  ([\#398](https://github.com/ropensci/targets/issues/398),
  [\#399](https://github.com/ropensci/targets/issues/399),
  [\#404](https://github.com/ropensci/targets/issues/404),
  [@pat-s](https://github.com/pat-s)).

### Enhancements

- Attempt to print only the useful part of the traceback in
  [`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md)
  ([\#383](https://github.com/ropensci/targets/issues/383)).
- Add a line break at the end of the “summary” reporter so warnings do
  not mangle the output.
- In
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
  use `shinybusy` instead of `shinycssloaders` and keep current output
  on display while new output is rendering
  ([\#386](https://github.com/ropensci/targets/issues/386),
  [@rcorty](https://github.com/rcorty)).
- Right-align the headers and counts in the “summary” and “forecast”
  reporters.
- Add a timestamp to the “summary” reporter.
- Make the reporters show when a target ends
  ([\#391](https://github.com/ropensci/targets/issues/391),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Make the reporters show when a pattern ends if the pattern built at
  least one target and none of the targets errored or canceled.
- Use words “start” and “built” in reporters.
- Use the region of the AWS S3 bucket instead of the local
  `AWS_DEFAULT_REGION` environment variable (`check_region = TRUE`;
  [\#400](https://github.com/ropensci/targets/issues/400),
  [@tomsing1](https://github.com/tomsing1)).
- In
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md),
  return `POSIXct` times in the time zone of the calling system
  ([\#131](https://github.com/ropensci/targets/issues/131)).
- Throw informative error messages when a target’s name or command is
  missing ([\#413](https://github.com/ropensci/targets/issues/413),
  [@liutiming](https://github.com/liutiming)).
- Bring back ALTREP in `qs::qread()` now that `qs` 0.24.1 requires
  `stringfish` \>= 1.5.0
  ([\#147](https://github.com/ropensci/targets/issues/147),
  [@glep](https://github.com/glep)).
- Relax dynamic branching checks so `pattern = slice(...)` can take
  multiple indexes
  ([\#406](https://github.com/ropensci/targets/issues/406),
  [\#419](https://github.com/ropensci/targets/issues/419),
  [@djbirke](https://github.com/djbirke),
  [@alexgphayes](https://github.com/alexgphayes))

## targets 0.3.1

CRAN release: 2021-03-28

### Bug fixes

- `queue$enqueue()` is now `queue$prepend()` and always appends to the
  front of the queue
  ([\#371](https://github.com/ropensci/targets/issues/371)).

### Enhancements

- Throw a warning if `devtools::load_all()` or similar is detected
  inside `_targets.R`
  ([\#374](https://github.com/ropensci/targets/issues/374)).

### CRAN

- Skip `feather` and `parquet` tests on CRAN.

## targets 0.3.0

CRAN release: 2021-03-27

### Bug fixes

- Fix the “write target at cursor” RStudio addin and move cursor between
  the parentheses.

### New features

- Add a `backoff` option in
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  to set the maximum upper bound (seconds) for the polling interval
  ([\#333](https://github.com/ropensci/targets/issues/333)).
- Add a new
  [`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md)
  function to write a GitHub Actions workflow file for continuous
  deployment of data analysis pipelines
  ([\#339](https://github.com/ropensci/targets/issues/339),
  [@jaredlander](https://github.com/jaredlander)).
- Add a new `TAR_MAKE_REPORTER` environment variable to globally set the
  reporter of the `tar_make*()` functions
  ([\#345](https://github.com/ropensci/targets/issues/345),
  [@alexpghayes](https://github.com/alexpghayes)).
- Support new storage formats “feather”, “parquet”, “aws_feather”, and
  “aws_parquet”
  ([\#355](https://github.com/ropensci/targets/issues/355),
  [@riazarbi](https://github.com/riazarbi)).

### Performance

- Implement an exponential backoff algorithm for polling the priority
  queue in
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  and
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  ([\#333](https://github.com/ropensci/targets/issues/333)).
- In
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
  try to submit a target every time a worker is polled.
- In
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md),
  poll workers in order of target priority.
- Avoid the time delay in exiting on error (from
  <https://github.com/r-lib/callr/issues/185>).
- Clone target objects for the pipeline and scrape more `targets`
  internal objects out of the environment in order to avoid accidental
  massive data transfers to workers.

### Enhancements

- Use
  [`rlang::check_installed()`](https://rlang.r-lib.org/reference/is_installed.html)
  inside `assert_package()`
  ([\#331](https://github.com/ropensci/targets/issues/331),
  [@malcolmbarrett](https://github.com/malcolmbarrett)).
- Allow `tar_destroy(destroy = "process")`.
- In
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
  increase default `seconds` to 15 (previously 5).
- In
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
  debounce instead of throttle inputs.
- In
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md),
  add an action button to refresh the outputs.
- Always deduplicate metadata after
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
  Will help compute a cache key on GitHub Actions and similar services.
- Deprecate
  [`tar_deduplicate()`](https://docs.ropensci.org/targets/reference/tar_deduplicate.md)
  due to the item above.
- Reorder information in timestamped messages.
- Document RNG seed generation in
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md),
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md),
  and
  [`tar_seed()`](https://docs.ropensci.org/targets/reference/tar_seed.md)
  ([\#357](https://github.com/ropensci/targets/issues/357),
  [@alexpghayes](https://github.com/alexpghayes)).
- Switch meaning of `%||%` and `%|||%` to conform to historical
  precedent.
- Only show a command line spinner if `reporter = "silent"`
  ([\#364](https://github.com/ropensci/targets/issues/364),
  [@matthiasgomolka](https://github.com/matthiasgomolka)).
- Target and pipeline objects no longer have an `envir` element.

## targets 0.2.0

CRAN release: 2021-02-27

### Bug fixes

- In
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
  subset metadata to avoid accidental attempts to load global objects in
  `tidyselect` calls.
- Do not register a pattern as running unless an actual branch is about
  to start ([\#304](https://github.com/ropensci/targets/issues/304)).
- Use a name spec in
  [`vctrs::vec_c()`](https://vctrs.r-lib.org/reference/vec_c.html)
  ([\#320](https://github.com/ropensci/targets/issues/320),
  [@joelnitta](https://github.com/joelnitta)).

### New features

- Add a new `names` argument to
  [`tar_objects()`](https://docs.ropensci.org/targets/reference/tar_objects.md)
  and
  [`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md)
  with `tidyselect` functionality.
- Record info on the main process (PID, R version, `targets` version) in
  `_targets/meta/process` and write new functions
  [`tar_process()`](https://docs.ropensci.org/targets/reference/tar_process.md)
  and
  [`tar_pid()`](https://docs.ropensci.org/targets/reference/tar_pid.md)
  to retrieve the data
  ([\#291](https://github.com/ropensci/targets/issues/291),
  [\#292](https://github.com/ropensci/targets/issues/292)).
- Add a new `targets_only` argument to
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md).
- Add new functions
  [`tar_helper()`](https://docs.ropensci.org/targets/reference/tar_helper.md)
  and
  [`tar_helper_raw()`](https://docs.ropensci.org/targets/reference/tar_helper.md)
  to write general-purpose R scripts, using tidy evaluation for as a
  template mechanism
  ([\#290](https://github.com/ropensci/targets/issues/290),
  [\#291](https://github.com/ropensci/targets/issues/291),
  [\#292](https://github.com/ropensci/targets/issues/292),
  [\#306](https://github.com/ropensci/targets/issues/306)).
- Export functions to check the existence of various pieces of local
  storage:
  [`tar_exist_meta()`](https://docs.ropensci.org/targets/reference/tar_exist_meta.md),
  [`tar_exist_objects()`](https://docs.ropensci.org/targets/reference/tar_exist_objects.md),
  [`tar_exist_progress()`](https://docs.ropensci.org/targets/reference/tar_exist_progress.md),
  [`tar_exist_progress()`](https://docs.ropensci.org/targets/reference/tar_exist_progress.md),
  [`tar_exist_script()`](https://docs.ropensci.org/targets/reference/tar_exist_script.md)
  ([\#310](https://github.com/ropensci/targets/issues/310)).
- Add a new `supervise` argument to
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md).
- Add a new `complete_only` argument to
  [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md)
  to optionally return only complete rows (no `NA` values).
- Catch `callr` errors and refer users to the debugging chapter of the
  manual.

### Enhancements

- Improve error messages of invalid arguments
  ([\#298](https://github.com/ropensci/targets/issues/298),
  [@brunocarlin](https://github.com/brunocarlin)). Removes partial
  argument matching in most cases.
- By default, locally enable `crayon` if an only if the calling process
  is interactive
  ([\#302](https://github.com/ropensci/targets/issues/302),
  [@ginolhac](https://github.com/ginolhac)). Can still be disabled with
  `options(crayon.enabled = FALSE)` in `_targets.R`.
- Improve error handling and message for `format = "url"` when the HTTP
  response status code is not 200
  ([\#303](https://github.com/ropensci/targets/issues/303),
  [@petrbouchal](https://github.com/petrbouchal)).
- Add more `extras` packages to
  [`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md)
  (to support
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)).
- Show informative message instead of error in
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  if `_targets.R` does not exist.
- Clear up the documentation of the `names` argument of
  [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  ([\#314](https://github.com/ropensci/targets/issues/314),
  [@jameelalsalam](https://github.com/jameelalsalam)).
- Do not override `nobody` in custom `curl` handles
  ([\#315](https://github.com/ropensci/targets/issues/315),
  [@riazarbi](https://github.com/riazarbi)).
- Rename “running” to “started” in the progress metadata. This avoids
  the implicit claim that `targets` is somehow actively monitoring each
  job, e.g. through a connection or heartbeat
  ([\#318](https://github.com/ropensci/targets/issues/318)).
- Set `errormode = "warn"` in `getVDigest()` for files to work around
  <https://github.com/eddelbuettel/digest/issues/49> for network drives
  on Windows. `targets` already runs those file checks anyway.
  ([\#316](https://github.com/ropensci/targets/issues/316),
  [@boshek](https://github.com/boshek)).
- If a package fails to load, print the library paths `targets` tried to
  load from.

## targets 0.1.0

CRAN release: 2021-02-01

### Bug fixes

- [`tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.md)
  now skips all tests on Solaris in order to fix the problems shown on
  the CRAN check page.
- Enable `allow` and `exclude` to work on imports in
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  and
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md).
- Put `visNetwork` legends on right to avoid crowding the graph.

### Performance

- Call [`force()`](https://rdrr.io/r/base/force.html) on subpipeline
  objects to eliminate high-memory promises in target objects. Allows
  targets to be deployed to workers much faster when `retreival` is
  `"main"` ([\#279](https://github.com/ropensci/targets/issues/279)).

### New features

- Add a new box to the
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  app to tabulate progress on dynamic branches
  ([\#273](https://github.com/ropensci/targets/issues/273),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Store `type`, `parent`, and `branches` in progress data for
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  ([\#273](https://github.com/ropensci/targets/issues/273),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Add a `fields` argument in
  [`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
  and default to `"progress"` for back compatibility
  ([\#273](https://github.com/ropensci/targets/issues/273),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Add a new
  [`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md)
  function to tabulate branch progress
  ([\#273](https://github.com/ropensci/targets/issues/273),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Add new “refresh” switch to
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  to toggle automatic refreshing and force a refresh.

### Enhancements

- Exclude `.Random.seed` by default in
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md).
- Spelling: “cancelled” changed to “canceled”.
- Enhance controls and use of space in the
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  app.
- Centralize internal path management utilities.

### Configuration

- Skip `clustermq` tests on Solaris.

## targets 0.0.2

CRAN release: 2021-01-21

### CRAN response

- Avoid starting the description with the package name.
- Remove `if(FALSE)` blocks from help files to fix “unexecutable code”
  warnings
  ([`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md),
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md),
  and
  [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)).
- Remove commented code in the examples
  ([`tar_edit()`](https://docs.ropensci.org/targets/reference/tar_edit.md),
  [`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md),
  and
  [`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md)).
- Ensure that all examples, tests, and vignettes do not write to the
  user’s home file space. (Fixed an example of
  [`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md).)

### Enhancements

- Use JOSS paper in `CITATION`.

## targets 0.0.1

### Enhancements

- Accept lists of target objects at the end of `_targets.R`
  ([\#253](https://github.com/ropensci/targets/issues/253)).
- Deprecate
  [`tar_pipeline()`](https://docs.ropensci.org/targets/reference/tar_pipeline.md)
  and
  [`tar_bind()`](https://docs.ropensci.org/targets/reference/tar_bind.md)
  because of the above
  ([\#253](https://github.com/ropensci/targets/issues/253)).
- Always show a special message when the pipeline finishes
  ([\#258](https://github.com/ropensci/targets/issues/258),
  [@petrbouchal](https://github.com/petrbouchal)).
- Disable `visNetwork` stabilization
  ([\#264](https://github.com/ropensci/targets/issues/264),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Use default `visNetwork` font size.
- Relay errors as condition messages if `error` is `"continue"`
  ([\#267](https://github.com/ropensci/targets/issues/267),
  [@liutiming](https://github.com/liutiming)).

## targets 0.0.0.9003

### Bug fixes

- Ensure pattern-only pipelines can be defined so they can be combined
  again later with
  [`tar_bind()`](https://docs.ropensci.org/targets/reference/tar_bind.md)
  ([\#245](https://github.com/ropensci/targets/issues/245),
  [@yonicd](https://github.com/yonicd)).
- Implement safeguards around `igraph` topological sort.

### Enhancements

- Topologically sort the rows of
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
  ([\#263](https://github.com/ropensci/targets/issues/263),
  [@sctyner](https://github.com/sctyner)).

### Breaking changes

- Make patterns composable
  ([\#212](https://github.com/ropensci/targets/issues/212),
  [@glep](https://github.com/glep),
  [@djbirke](https://github.com/djbirke)).
- Allow workspaces to load nonexportable objects
  ([\#214](https://github.com/ropensci/targets/issues/214)).
- Make workspace files super light by saving only a reference to the
  required dependencies
  ([\#214](https://github.com/ropensci/targets/issues/214)).
- Add a new `workspaces` argument to
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  to specify which targets will save their workspace files during
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  ([\#214](https://github.com/ropensci/targets/issues/214)).
- Change `error = "save"` to `error = "workspace"` to so it is clearer
  that saving workspaces no longer duplicates data
  ([\#214](https://github.com/ropensci/targets/issues/214)).
- Rename `what` to `destroy` in
  [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md).
- Remove `tar_undebug()` because is redundant with
  `tar_destroy(destroy = "workspaces")`.

### New features

- Make patterns composable
  ([\#212](https://github.com/ropensci/targets/issues/212)).
- Add new dynamic branching patterns
  [`head()`](https://docs.ropensci.org/targets/reference/tar_pattern.md),
  [`tail()`](https://docs.ropensci.org/targets/reference/tar_pattern.md),
  and
  [`sample()`](https://docs.ropensci.org/targets/reference/tar_pattern.md)
  to provide functionality equivalent to `drake`’s `max_expand`
  ([\#56](https://github.com/ropensci/targets/issues/56)).
- Add a new
  [`tar_pattern()`](https://docs.ropensci.org/targets/reference/tar_pattern.md)
  function to emulate dynamic branching outside a pipeline.
- Add a new `level_separation` argument to
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  and
  [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  to control the aspect ratio
  ([\#226](https://github.com/ropensci/targets/issues/226)).
- Track functions from multiple packages with the `imports` argument to
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  ([\#239](https://github.com/ropensci/targets/issues/239)).
- Add color for “built” progress if `outdated` is `FALSE` in
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md).
- Tweak colors in
  [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  to try to account for color blindness.

### Enhancements

- Return full patterns from
  [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md).
- Record package load errors in progress and metadata
  ([\#228](https://github.com/ropensci/targets/issues/228),
  [@psychelzh](https://github.com/psychelzh)).
- [`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md)
  now invokes `_targets.R` through a background process just like
  [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
  etc. so it can account for more hidden packages
  ([\#224](https://github.com/ropensci/targets/issues/224),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Set `deployment` equal to `"main"` for all targets in
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md).
  This ensures
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  does not waste time waiting for nonexistent files to ship over a
  nonexistent network file system (NFS).
  [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  or
  [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  could use NFS, so they still leave `deployment` alone.

## targets 0.0.0.9002

### Breaking changes

- Add a new `size` field to the metadata to allow `targets` to make
  better judgments about when to rehash files
  ([\#180](https://github.com/ropensci/targets/issues/180)). We now
  compare hashes to check file size differences instead of doing messy
  floating point comparisons with ad hoc tolerances. It breaks back
  compatibility with old projects, but the error message is informative,
  and this is all still before the first official release.
- Change “local” to “main” and “remote” to “worker” in the `storage`,
  `retrieval`, and `deployment` settings
  ([\#183](https://github.com/ropensci/targets/issues/183),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Ensure function dependencies are sorted before computing the function
  hash (GitHub commit f15face7d72c15c2d1098da959492bdbfcddb425).
- Move `garbage_collection` to a target-level setting, i.e. argument to
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  and
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  ([\#194](https://github.com/ropensci/targets/issues/194)). Previously
  was an argument to the `tar_make*()` functions.
- Allow
  [`tar_name()`](https://docs.ropensci.org/targets/reference/tar_name.md)
  and
  [`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md)
  to run outside the pipeline with debugging-friendly default return
  values.

### Bug fixes

- Stop sending target return values over the network when `storage` is
  `"remote"` ([\#182](https://github.com/ropensci/targets/issues/182),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Shorten lengths of warnings and error messages to 128 characters
  ([\#186](https://github.com/ropensci/targets/issues/186),
  [@gorkang](https://github.com/gorkang)).
- Restrict in-memory metadata to avoid incorrectly recycling deleted
  targets ([\#191](https://github.com/ropensci/targets/issues/191)).
- Marshal nonexportable dependencies before sending them to workers.
  Transport data through `target$subpipeline` rather than `target$cache`
  to make that happen
  ([\#209](https://github.com/ropensci/targets/issues/209),
  [@mattwarkentin](https://github.com/mattwarkentin)).

### New features

- Add a new function
  [`tar_bind()`](https://docs.ropensci.org/targets/reference/tar_bind.md)
  to combine pipeline objects.
- Add
  [`tar_seed()`](https://docs.ropensci.org/targets/reference/tar_seed.md)
  to get the random number generator seed of the target currently
  running.

### Enhancements

- Allow target-specific
  [`future::plan()`](https://future.futureverse.org/reference/plan.html)s
  through the `resources` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  ([\#198](https://github.com/ropensci/targets/issues/198),
  [@mattwarkentin](https://github.com/mattwarkentin)).
- Use [`library()`](https://rdrr.io/r/base/library.html) instead of
  [`require()`](https://rdrr.io/r/base/library.html) in
  `command_load_packages()`.
- Evaluate commands directly in `targets$cache$targets$envir` to improve
  convenience in interactive debugging
  ([`ls()`](https://rdrr.io/r/base/ls.html) just works now.) This is
  reasonably safe now that the cache is populated at the last minute and
  cleared as soon as possible
  ([\#209](https://github.com/ropensci/targets/issues/209),
  [\#210](https://github.com/ropensci/targets/issues/210)).

## targets 0.0.0.9000

- First version.
