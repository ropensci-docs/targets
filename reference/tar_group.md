# Group a data frame to iterate over subsets of rows.

Like
[`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html),
but for patterns. `tar_group()` allows you to map or cross over subsets
of data frames. Requires `iteration = "group"` on the target. See the
example.

## Usage

``` r
tar_group(x)
```

## Arguments

- x:

  Grouped data frame from
  [`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html)

## Value

A data frame with a special `tar_group` column that `targets` will use
to find subsets of your data frame.

## Details

The goal of `tar_group()` is to post-process the return value of a data
frame target to allow downstream targets to branch over subsets of rows.
It takes the groups defined by
[`dplyr::group_by()`](https://dplyr.tidyverse.org/reference/group_by.html)
and translates that information into a special `tar_group` is a column.
`tar_group` is a vector of positive integers from 1 to the number of
groups. Rows with the same integer in `tar_group` belong to the same
group, and branches are arranged in increasing order with respect to the
integers in `tar_group`. The assignment of `tar_group` integers to group
levels depends on the orderings inside the grouping variables and not
the order of rows in the dataset.
[`dplyr::group_keys()`](https://dplyr.tidyverse.org/reference/group_data.html)
on the grouped data frame shows how the grouping variables correspond to
the integers in the `tar_group` column.

## See also

Other utilities:
[`tar_active()`](https://docs.ropensci.org/targets/reference/tar_active.md),
[`tar_backoff()`](https://docs.ropensci.org/targets/reference/tar_backoff.md),
[`tar_call()`](https://docs.ropensci.org/targets/reference/tar_call.md),
[`tar_cancel()`](https://docs.ropensci.org/targets/reference/tar_cancel.md),
[`tar_definition()`](https://docs.ropensci.org/targets/reference/tar_definition.md),
[`tar_described_as()`](https://docs.ropensci.org/targets/reference/tar_described_as.md),
[`tar_envir()`](https://docs.ropensci.org/targets/reference/tar_envir.md),
[`tar_format_get()`](https://docs.ropensci.org/targets/reference/tar_format_get.md),
[`tar_name()`](https://docs.ropensci.org/targets/reference/tar_name.md),
[`tar_path()`](https://docs.ropensci.org/targets/reference/tar_path.md),
[`tar_path_script()`](https://docs.ropensci.org/targets/reference/tar_path_script.md),
[`tar_path_script_support()`](https://docs.ropensci.org/targets/reference/tar_path_script_support.md),
[`tar_path_store()`](https://docs.ropensci.org/targets/reference/tar_path_store.md),
[`tar_path_target()`](https://docs.ropensci.org/targets/reference/tar_path_target.md),
[`tar_source()`](https://docs.ropensci.org/targets/reference/tar_source.md),
[`tar_store()`](https://docs.ropensci.org/targets/reference/tar_store.md),
[`tar_unblock_process()`](https://docs.ropensci.org/targets/reference/tar_unblock_process.md)

## Examples

``` r
if (identical(Sys.getenv("TAR_EXAMPLES"), "true")) { # for CRAN
# The tar_group() function simply creates
# a tar_group column to partition the rows
# of a data frame.
data.frame(
  x = seq_len(6),
  id = rep(letters[seq_len(3)], each = 2)
) %>%
  dplyr::group_by(id) %>%
  tar_group()
# We use tar_group() below to branch over
# subsets of a data frame defined with dplyr::group_by().
tar_dir({ # tar_dir() runs code from a temp dir for CRAN.
tar_script({
library(dplyr)
library(targets)
library(tarchetypes)
list(
  tar_target(
    data,
    data.frame(
      x = seq_len(6),
      id = rep(letters[seq_len(3)], each = 2)
    ) %>%
      group_by(id) %>%
      tar_group(),
    iteration = "group"
  ),
  tar_target(
    sums,
    sum(data$x),
    pattern = map(data),
    iteration = "vector"
  )
)
})
tar_make()
tar_read(sums) # Should be c(3, 7, 11).
})
}
```
