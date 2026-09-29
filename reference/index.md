# Package index

## Help

- [`targets-package`](https://docs.ropensci.org/targets/reference/targets-package.md)
  : targets: Dynamic Function-Oriented Make-Like Declarative Pipelines
  for R

- [`use_targets()`](https://docs.ropensci.org/targets/reference/use_targets.md)
  : Use targets

- [`use_targets_rmd()`](https://docs.ropensci.org/targets/reference/use_targets_rmd.md)
  : Use targets with Target Markdown.

- [`tar_reprex()`](https://docs.ropensci.org/targets/reference/tar_reprex.md)
  :

  Reproducible example of `targets` with `reprex`

## Scripts

- [`tar_edit()`](https://docs.ropensci.org/targets/reference/tar_edit.md)
  : Open the target script file for editing.

- [`tar_github_actions()`](https://docs.ropensci.org/targets/reference/tar_github_actions.md)
  : Set up GitHub Actions to run a targets pipeline

- [`tar_helper()`](https://docs.ropensci.org/targets/reference/tar_helper.md)
  [`tar_helper_raw()`](https://docs.ropensci.org/targets/reference/tar_helper.md)
  : Write a helper R script.

- [`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md)
  :

  Set up package dependencies for compatibility with `renv`

- [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)
  : Write a target script file.

## Configuration

- [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.md)
  : Get configuration settings.

- [`tar_config_projects()`](https://docs.ropensci.org/targets/reference/tar_config_projects.md)
  : List projects.

- [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.md)
  : Set configuration settings.

- [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.md)
  : Unset configuration settings.

- [`tar_config_yaml()`](https://docs.ropensci.org/targets/reference/tar_config_yaml.md)
  :

  Read `_targets.yaml`.

- [`tar_envvars()`](https://docs.ropensci.org/targets/reference/tar_envvars.md)
  :

  Show `targets` environment variables.

- [`tar_option_get()`](https://docs.ropensci.org/targets/reference/tar_option_get.md)
  : Get a target option.

- [`tar_option_reset()`](https://docs.ropensci.org/targets/reference/tar_option_reset.md)
  : Reset all target options.

- [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  : Set target options.

- [`tar_option_unset()`](https://docs.ropensci.org/targets/reference/tar_option_unset.md)
  : Unset one or more target options.

- [`tar_option_with()`](https://docs.ropensci.org/targets/reference/tar_option_with.md)
  : Locally set target options.

## Targets

- [`tar_cue()`](https://docs.ropensci.org/targets/reference/tar_cue.md)
  : Declare the rules that cue a target.
- [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  [`tar_target_raw()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  : Declare a target.

## Pipeline

- [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
  : Run a pipeline of targets.

- [`tar_make_clustermq()`](https://docs.ropensci.org/targets/reference/tar_make_clustermq.md)
  :

  Superseded. Run a pipeline with persistent `clustermq` workers.

- [`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md)
  :

  Superseded. Run a pipeline of targets in parallel with transient
  `future` workers.

## Debug

- [`tar_load_globals()`](https://docs.ropensci.org/targets/reference/tar_load_globals.md)
  : Load globals for debugging, testing, and prototyping
- [`tar_traceback()`](https://docs.ropensci.org/targets/reference/tar_traceback.md)
  : Get a target's traceback
- [`tar_workspace()`](https://docs.ropensci.org/targets/reference/tar_workspace.md)
  : Load a locally saved workspace and seed for debugging.
- [`tar_workspace_download()`](https://docs.ropensci.org/targets/reference/tar_workspace_download.md)
  : Download a workspace from the cloud.
- [`tar_workspaces()`](https://docs.ropensci.org/targets/reference/tar_workspaces.md)
  : List locally saved target workspaces.

## Storage

- [`tar_format()`](https://docs.ropensci.org/targets/reference/tar_format.md)
  : Define a custom target storage format.
- [`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  [`tar_load_raw()`](https://docs.ropensci.org/targets/reference/tar_load.md)
  : Load the values of targets.
- [`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md)
  : Load the values of all available targets.
- [`tar_objects()`](https://docs.ropensci.org/targets/reference/tar_objects.md)
  : List saved targets
- [`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  [`tar_read_raw()`](https://docs.ropensci.org/targets/reference/tar_read.md)
  : Read a target's value from storage.

## Content-addressable storage

- [`tar_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_repository_cas.md)
  : Define a custom content-addressable storage (CAS) repository (an
  experimental feature).
- [`tar_repository_cas_local()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local.md)
  : Local content-addressable storage (CAS) repository (an experimental
  feature).
- [`tar_repository_cas_local_gc()`](https://docs.ropensci.org/targets/reference/tar_repository_cas_local_gc.md)
  : Local CAS garbage collection

## Metadata

- [`tar_crew()`](https://docs.ropensci.org/targets/reference/tar_crew.md)
  : Get crew worker info.
- [`tar_meta()`](https://docs.ropensci.org/targets/reference/tar_meta.md)
  : Read a project's metadata.
- [`tar_meta_delete()`](https://docs.ropensci.org/targets/reference/tar_meta_delete.md)
  : Delete metadata.
- [`tar_meta_download()`](https://docs.ropensci.org/targets/reference/tar_meta_download.md)
  : download local metadata to the cloud.
- [`tar_meta_sync()`](https://docs.ropensci.org/targets/reference/tar_meta_sync.md)
  : Synchronize cloud metadata.
- [`tar_meta_upload()`](https://docs.ropensci.org/targets/reference/tar_meta_upload.md)
  : Upload local metadata to the cloud.
- [`tar_pid()`](https://docs.ropensci.org/targets/reference/tar_pid.md)
  : Get main process ID.
- [`tar_process()`](https://docs.ropensci.org/targets/reference/tar_process.md)
  : Get main process info.

## Inspect

- [`tar_deps()`](https://docs.ropensci.org/targets/reference/tar_deps.md)
  [`tar_deps_raw()`](https://docs.ropensci.org/targets/reference/tar_deps.md)
  : Code dependencies
- [`tar_igraph()`](https://docs.ropensci.org/targets/reference/tar_igraph.md)
  : Get the igraph.
- [`tar_manifest()`](https://docs.ropensci.org/targets/reference/tar_manifest.md)
  : Produce a data frame of information about your targets.
- [`tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.md)
  : Check which targets are outdated.
- [`tar_sitrep()`](https://docs.ropensci.org/targets/reference/tar_sitrep.md)
  : Show the cue-by-cue status of each target.
- [`tar_validate()`](https://docs.ropensci.org/targets/reference/tar_validate.md)
  : Validate a pipeline of targets.

## Visualize

- [`tar_glimpse()`](https://docs.ropensci.org/targets/reference/tar_glimpse.md)
  : Visualize an abridged fast dependency graph.

- [`tar_mermaid()`](https://docs.ropensci.org/targets/reference/tar_mermaid.md)
  :

  `mermaid.js` dependency graph.

- [`tar_network()`](https://docs.ropensci.org/targets/reference/tar_network.md)
  : Return the vertices and edges of a pipeline dependency graph.

- [`tar_visnetwork()`](https://docs.ropensci.org/targets/reference/tar_visnetwork.md)
  : visNetwork dependency graph.

## Clean

- [`tar_delete()`](https://docs.ropensci.org/targets/reference/tar_delete.md)
  : Delete target output values.

- [`tar_destroy()`](https://docs.ropensci.org/targets/reference/tar_destroy.md)
  : Destroy the data store.

- [`tar_invalidate()`](https://docs.ropensci.org/targets/reference/tar_invalidate.md)
  : Delete one or more metadata records (e.g. to rerun a target).

- [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
  : Remove targets that are no longer part of the pipeline.

- [`tar_prune_list()`](https://docs.ropensci.org/targets/reference/tar_prune_list.md)
  :

  List targets that
  [`tar_prune()`](https://docs.ropensci.org/targets/reference/tar_prune.md)
  will remove.

- [`tar_unscript()`](https://docs.ropensci.org/targets/reference/tar_unscript.md)
  : Remove target script helper files.

- [`tar_unversion()`](https://docs.ropensci.org/targets/reference/tar_unversion.md)
  : Delete cloud object version IDs from local metadata.

## Progress

- [`tar_poll()`](https://docs.ropensci.org/targets/reference/tar_poll.md)
  : Repeatedly poll progress in the R console.
- [`tar_watch()`](https://docs.ropensci.org/targets/reference/tar_watch.md)
  : Shiny app to watch the dependency graph.
- [`tar_watch_server()`](https://docs.ropensci.org/targets/reference/tar_watch_server.md)
  : Shiny module server for tar_watch()
- [`tar_watch_ui()`](https://docs.ropensci.org/targets/reference/tar_watch_ui.md)
  : Shiny module UI for tar_watch()
- [`tar_progress()`](https://docs.ropensci.org/targets/reference/tar_progress.md)
  : Read progress.
- [`tar_progress_branches()`](https://docs.ropensci.org/targets/reference/tar_progress_branches.md)
  : Tabulate the progress of dynamic branches.
- [`tar_progress_summary()`](https://docs.ropensci.org/targets/reference/tar_progress_summary.md)
  : Summarize target progress.
- [`tar_skipped()`](https://docs.ropensci.org/targets/reference/tar_skipped.md)
  : List skipped targets.
- [`tar_dispatched()`](https://docs.ropensci.org/targets/reference/tar_dispatched.md)
  : List dispatched targets.
- [`tar_completed()`](https://docs.ropensci.org/targets/reference/tar_completed.md)
  : List completed targets.
- [`tar_canceled()`](https://docs.ropensci.org/targets/reference/tar_canceled.md)
  : List canceled targets.
- [`tar_errored()`](https://docs.ropensci.org/targets/reference/tar_errored.md)
  : List errored targets.

## Time

- [`tar_newer()`](https://docs.ropensci.org/targets/reference/tar_newer.md)
  : List new targets
- [`tar_older()`](https://docs.ropensci.org/targets/reference/tar_older.md)
  : List old targets
- [`tar_timestamp()`](https://docs.ropensci.org/targets/reference/tar_timestamp.md)
  [`tar_timestamp_raw()`](https://docs.ropensci.org/targets/reference/tar_timestamp.md)
  : Get the timestamp(s) of a target.

## Existence

- [`tar_exist_meta()`](https://docs.ropensci.org/targets/reference/tar_exist_meta.md)
  : Check if target metadata exists.
- [`tar_exist_objects()`](https://docs.ropensci.org/targets/reference/tar_exist_objects.md)
  : Check if local output data exists for one or more targets.
- [`tar_exist_process()`](https://docs.ropensci.org/targets/reference/tar_exist_process.md)
  : Check if process metadata exists.
- [`tar_exist_progress()`](https://docs.ropensci.org/targets/reference/tar_exist_progress.md)
  : Check if progress metadata exists.
- [`tar_exist_script()`](https://docs.ropensci.org/targets/reference/tar_exist_script.md)
  : Check if the target script file exists.

## Branching

- [`tar_branch_index()`](https://docs.ropensci.org/targets/reference/tar_branch_index.md)
  : Integer branch indexes
- [`tar_branch_names()`](https://docs.ropensci.org/targets/reference/tar_branch_names.md)
  [`tar_branch_names_raw()`](https://docs.ropensci.org/targets/reference/tar_branch_names.md)
  : Branch names
- [`tar_branches()`](https://docs.ropensci.org/targets/reference/tar_branches.md)
  : Reconstruct the branch names and the names of their dependencies.
- [`tar_pattern()`](https://docs.ropensci.org/targets/reference/tar_pattern.md)
  : Emulate dynamic branching.

## Resources

- [`tar_resources()`](https://docs.ropensci.org/targets/reference/tar_resources.md)
  : Target resources

- [`tar_resources_aws()`](https://docs.ropensci.org/targets/reference/tar_resources_aws.md)
  : Target resources: Amazon Web Services (AWS) S3 storage

- [`tar_resources_clustermq()`](https://docs.ropensci.org/targets/reference/tar_resources_clustermq.md)
  :

  Target resources: `clustermq` high-performance computing

- [`tar_resources_crew()`](https://docs.ropensci.org/targets/reference/tar_resources_crew.md)
  :

  Target resources: `crew` high-performance computing

- [`tar_resources_custom_format()`](https://docs.ropensci.org/targets/reference/tar_resources_custom_format.md)
  : Target resources for custom storage formats

- [`tar_resources_feather()`](https://docs.ropensci.org/targets/reference/tar_resources_feather.md)
  : Target resources: feather storage formats

- [`tar_resources_fst()`](https://docs.ropensci.org/targets/reference/tar_resources_fst.md)
  :

  Target resources: `fst` storage formats

- [`tar_resources_future()`](https://docs.ropensci.org/targets/reference/tar_resources_future.md)
  :

  Target resources: `future` high-performance computing

- [`tar_resources_gcp()`](https://docs.ropensci.org/targets/reference/tar_resources_gcp.md)
  : Target resources: Google Cloud Platform (GCP) Google Cloud Storage
  (GCS)

- [`tar_resources_network()`](https://docs.ropensci.org/targets/reference/tar_resources_network.md)
  : Target resources for network file systems.

- [`tar_resources_parquet()`](https://docs.ropensci.org/targets/reference/tar_resources_parquet.md)
  : Target resources: parquet storage formats

- [`tar_resources_qs()`](https://docs.ropensci.org/targets/reference/tar_resources_qs.md)
  : Target resources: qs storage formats

- [`tar_resources_repository_cas()`](https://docs.ropensci.org/targets/reference/tar_resources_repository_cas.md)
  : Target resources for custom storage formats

- [`tar_resources_url()`](https://docs.ropensci.org/targets/reference/tar_resources_url.md)
  : Target resources: URL storage formats

## Target Markdown

- [`tar_engine_knitr()`](https://docs.ropensci.org/targets/reference/tar_engine_knitr.md)
  :

  Target Markdown `knitr` engine

- [`tar_interactive()`](https://docs.ropensci.org/targets/reference/tar_interactive.md)
  : Run if Target Markdown interactive mode is on.

- [`tar_noninteractive()`](https://docs.ropensci.org/targets/reference/tar_noninteractive.md)
  : Run if Target Markdown interactive mode is not on.

- [`tar_toggle()`](https://docs.ropensci.org/targets/reference/tar_toggle.md)
  : Choose code to run based on Target Markdown mode.

## Pseudo-random number generation

- [`tar_seed_create()`](https://docs.ropensci.org/targets/reference/tar_seed_create.md)
  : Create a seed for a target.
- [`tar_seed_get()`](https://docs.ropensci.org/targets/reference/tar_seed_get.md)
  : Get the random number generator seed of the target currently
  running.
- [`tar_seed_set()`](https://docs.ropensci.org/targets/reference/tar_seed_set.md)
  : Set a seed to run a target.

## Utilities

- [`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md)
  : Show if the pipeline is running.

- [`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md)
  : Superseded: exponential backoff

- [`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md)
  :

  Identify the called `targets` function.

- [`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md)
  : Cancel a target mid-execution under a custom condition.

- [`tar_definition()`](https://docs.ropensci.org/targets/reference/tar_definition.md)
  : For developers only: get the definition of the current target.

- [`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md)
  : Select targets using their descriptions.

- [`tar_envir()`](https://docs.ropensci.org/targets/reference/tar_envir.md)
  : For developers only: get the environment of the current target.

- [`tar_format_get()`](https://docs.ropensci.org/targets/reference/tar_format_get.md)
  : Current storage format.

- [`tar_group()`](https://docs.ropensci.org/targets/reference/tar_group.md)
  : Group a data frame to iterate over subsets of rows.

- [`tar_name()`](https://docs.ropensci.org/targets/reference/tar_name.md)
  : Get the name of the target currently running.

- [`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md)
  : Current target script path

- [`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md)
  : Directory path to the support scripts of the current target script

- [`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md)
  : Current data store path

- [`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md)
  : Identify the file path where the current target will be stored.

- [`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md)
  : Run R scripts.

- [`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)
  : Unblock the pipeline process

## Extending targets

- [`tar_assert_chr()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_dbl()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_df()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_equal_lengths()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_envir()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_expr()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_flag()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_file()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_finite()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_function()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_function_arguments()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_ge()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_identical()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_in()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_not_dirs()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_not_dir()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_not_in()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_inherits()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_int()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_internet()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_lang()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_le()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_list()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_lgl()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_name()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_named()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_names()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_nonempty()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_null()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_not_expr()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_nzchar()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_package()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_path()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_match()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_nonmissing()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_positive()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_scalar()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_store()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_target()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_target_list()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_true()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_unique()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  [`tar_assert_unique_targets()`](https://docs.ropensci.org/targets/reference/tar_assert.md)
  : Assertions
- [`tar_message_run()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_throw_file()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_throw_run()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_throw_validate()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_warn_deprecate()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_warn_run()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_warn_validate()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_message_validate()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_print()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_error()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_warning()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  [`tar_message()`](https://docs.ropensci.org/targets/reference/tar_condition.md)
  : Conditions
- [`tar_deparse_language()`](https://docs.ropensci.org/targets/reference/tar_language.md)
  [`tar_deparse_safe()`](https://docs.ropensci.org/targets/reference/tar_language.md)
  [`tar_tidy_eval()`](https://docs.ropensci.org/targets/reference/tar_language.md)
  [`tar_tidyselect_eval()`](https://docs.ropensci.org/targets/reference/tar_language.md)
  : Language
- [`tar_test()`](https://docs.ropensci.org/targets/reference/tar_test.md)
  : Test code in a temporary directory.
