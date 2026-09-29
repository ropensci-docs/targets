# Set up GitHub Actions to run a targets pipeline

Writes a GitHub Actions workflow file so the pipeline runs on every push
to GitHub. Historical runs accumulate in the `targets-runs` branch, and
the latest output is restored before
[`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.md)
so up-to-date targets do not rerun.

## Usage

``` r
tar_github_actions(
  path = file.path(".github", "workflows", "targets.yaml"),
  ask = NULL
)
```

## Arguments

- path:

  Character of length 1, file path to write the GitHub Actions workflow
  file.

- ask:

  Logical, whether to ask before writing if the workflow file already
  exists. If `NULL`, defaults to `Sys.getenv("TAR_ASK")`. (Set to
  `"true"` or `"false"` with
  [`Sys.setenv()`](https://rdrr.io/r/base/Sys.setenv.html)). If `ask`
  and the `TAR_ASK` environment variable are both indeterminate,
  defaults to
  [`interactive()`](https://rdrr.io/r/base/interactive.html).

## Value

Nothing (invisibly). This function writes a GitHub Actions workflow file
as a side effect.

## Details

Steps to set up continuous deployment:

1.  Ensure your pipeline stays within the resource limitations of GitHub
    Actions and repositories, both for storage and compute. For storage,
    you may wish to reduce the burden with an alternative repository
    (e.g. `tar_target(..., repository = "aws")`).

2.  Ensure Actions are enabled in your GitHub repository. You may have
    to visit the Settings tab.

3.  Call `targets::tar_renv(extras = character(0))` to expose hidden
    package dependencies.

4.  Set up `renv` for your project (with `renv::init()` or
    `renv::snapshot()`). Details at
    <https://rstudio.github.io/renv/articles/ci.html>.

5.  Commit the `renv.lock` file to the `main` (recommended) or `master`
    Git branch.

6.  Run `tar_github_actions()` to create the workflow file. Commit this
    file to `main` (recommended) or `master` in Git.

7.  Push your project to GitHub. Verify that a GitHub Actions workflow
    runs and pushes results to `targets-runs`. Subsequent runs will only
    recompute the outdated targets.

## See also

Other scripts:
[`tar_edit()`](https://docs.ropensci.org/targets/reference/tar_edit.md),
[`tar_helper()`](https://docs.ropensci.org/targets/reference/tar_helper.md),
[`tar_renv()`](https://docs.ropensci.org/targets/reference/tar_renv.md),
[`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.md)

## Examples

``` r
tar_github_actions(tempfile())
```
