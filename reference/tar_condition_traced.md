# Contain an error condition and formatted traceback.

Not a user-side function.

## Usage

``` r
tar_condition_traced(condition, trace)
```

## Arguments

- condition:

  An error condition object thrown by
  [`stop()`](https://rdrr.io/r/base/stop.html) or
  [`rlang::abort()`](https://rlang.r-lib.org/reference/abort.html).

- trace:

  A raw return value from
  [`.traceback()`](https://rdrr.io/r/base/traceback.html).

## Value

Contain an error condition and formatted traceback.
