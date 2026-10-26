# Define a custom target storage format.

Define a custom target storage format for the `format` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
or
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).

## Usage

``` r
tar_format(
  read = NULL,
  write = NULL,
  marshal = NULL,
  unmarshal = NULL,
  convert = NULL,
  copy = NULL,
  substitute = list(),
  repository = NULL
)
```

## Arguments

- read:

  A function with a single argument named `path`. This function should
  read and return the target stored at the file in the argument. It
  should have no side effects. See the "Format functions" section for
  specific requirements. If `NULL`, the `read` argument defaults to
  [`readRDS()`](https://rdrr.io/r/base/readRDS.html).

- write:

  A function with two arguments: `object` and `path`, in that order.
  This function should save the R object `object` to the file path at
  `path` and have no other side effects. The function need not return a
  value, but the file written to `path` must be a single file, and it
  cannot be a directory. See the "Format functions" section for specific
  requirements. If `NULL`, the `write` argument defaults to
  [`saveRDS()`](https://rdrr.io/r/base/readRDS.html) with `version = 3`.

- marshal:

  A function with a single argument named `object`. This function should
  marshal the R object and return an in-memory object that can be
  exported to remote parallel workers. It should not read or write any
  persistent files. See the Marshalling section for details. See the
  "Format functions" section for specific requirements. If `NULL`, the
  `marshal` argument defaults to just returning the original object
  without any modifications.

- unmarshal:

  A function with a single argument named `object`. This function should
  unmarshal the (marshalled) R object and return an in-memory object
  that is appropriate and valid for use on a parallel worker. It should
  not read or write any persistent files. See the Marshalling section
  for details. See the "Format functions" section for specific
  requirements. If `NULL`, the `unmarshal` argument defaults to just
  returning the original object without any modifications.

- convert:

  The `convert` argument is a function with a single argument named
  `object`. It accepts the object returned by the command of the target
  and changes it into an acceptable format (e.g. can be saved with the
  `read` function). The `convert` ensures the in-memory copy of an
  object during the running pipeline session is the same as the copy of
  the object that is saved to disk. The function should be idempotent,
  and it should handle edge cases like `NULL` values (especially for
  `error = "null"` in
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  or
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)).
  If `NULL`, the `convert` argument defaults to just returning the
  original object without any modifications.

- copy:

  The `copy` argument is a function with a single function named
  `object`. It accepts the object returned by the command of the target
  and makes a deep copy in memory. This method does is relevant to
  objects like `data.table`s that support in-place modification which
  could cause unpredictable side effects from target to target. In cases
  like these, the target should be deep-copied before a downstream
  target attempts to use it (in the case of `data.table` objects, using
  [`data.table::copy()`](https://rdrr.io/pkg/data.table/man/copy.html)).
  If `NULL`, the `copy` argument defaults to just returning the original
  object without any modifications.

- substitute:

  Named list of values to be inserted into the body of each custom
  function in place of symbols in the body. For example, if
  `write = function(object, path) saveRDS(object, path, version = VERSION)`
  and `substitute = list(VERSION = 3)`, then the `write` function will
  actually end up being
  `function(object, path) saveRDS(object, path, version = 3)`.

  Please do not include temporary or sensitive information such as
  authentication credentials. If you do, then `targets` will write them
  to metadata on disk, and a malicious actor could steal and misuse
  them. Instead, pass sensitive information as environment variables
  using
  [`tar_resources_custom_format()`](https://docs.ropensci.org/targets/reference/tar_resources_custom_format.md).
  These environment variables only exist in the transient memory spaces
  of the R sessions of the local and worker processes.

- repository:

  Deprecated. Use the `repository` argument of
  [`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
  or
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md)
  instead.

## Value

A character string of length 1 encoding the custom format. You can
supply this string directly to the `format` argument of
[`tar_target()`](https://docs.ropensci.org/targets/reference/tar_target.md)
or
[`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.md).

## Marshalling

If an object can only be used in the R session where it was created, it
is called "non-exportable". Examples of non-exportable R objects are
Keras models, Torch objects, `xgboost` matrices, `xml2` documents,
`rstan` model objects, `sparklyr` data objects, and database connection
objects. These objects cannot be exported to parallel workers (e.g. for
[`tar_make_future()`](https://docs.ropensci.org/targets/reference/tar_make_future.md))
without special treatment. To send an non-exportable object to a
parallel worker, the object must be marshalled: converted into a form
that can be exported safely (similar to serialization but not always the
same). Then, the worker must unmarshal the object: convert it into a
form that is usable and valid in the current R session. Arguments
`marshal` and `unmarshal` of `tar_format()` let you control how
marshalling and unmarshalling happens.

## Format functions

In `tar_format()`, functions like `read`, `write`, `marshal`, and
`unmarshal` must be perfectly pure and perfectly self-sufficient. They
must load or namespace all their own packages, and they must not depend
on any custom user-defined functions or objects in the global
environment of your pipeline. `targets` converts each function to and
from text, so it must not rely on any data in the closure. This
disqualifies functions produced by
[`Vectorize()`](https://rdrr.io/r/base/Vectorize.html), for example.

The `write` function must write only a single file, and the file it
writes must not be a directory.

The functions to read and write the object should not do any conversions
on the object. That is the job of the `convert` argument. The `convert`
argument is a function that accepts the object returned by the command
of the target and changes it into an acceptable format (e.g. can be
saved with the `read` function). Working with the `convert` function is
best because it ensures the in-memory copy of an object during the
running pipeline session is the same as the copy of the object that is
saved to disk.

## See also

Other storage:
[`tar_load()`](https://docs.ropensci.org/targets/reference/tar_load.md),
[`tar_load_everything()`](https://docs.ropensci.org/targets/reference/tar_load_everything.md),
[`tar_objects()`](https://docs.ropensci.org/targets/reference/tar_objects.md),
[`tar_read()`](https://docs.ropensci.org/targets/reference/tar_read.md)

## Examples

``` r
# The following target is equivalent to the current superseded
# tar_target(name, command(), format = "keras").
# An improved version of this would supply a `convert` argument
# to handle NULL objects, which are returned by the target if it
# errors and the error argument of tar_target() is "null".
tar_target(
  name = keras_target,
  command = your_function(),
  format = tar_format(
    read = function(path) {
      keras::load_model_hdf5(path)
    },
    write = function(object, path) {
      keras::save_model_hdf5(object = object, filepath = path)
    },
    marshal = function(object) {
      keras::serialize_model(object)
    },
    unmarshal = function(object) {
      keras::unserialize_model(object)
    }
  )
)
#> <tar_stem> 
#>   name: keras_target 
#>   description:  
#>   command:
#>     your_function() 
#>   format: format_custom&read=ewogICAga2VyYXM6OmxvYWRfbW9kZWxfaGRmNShwYXRoKQp9&write=ewogICAga2VyYXM6OnNhdmVfbW9kZWxfaGRmNShvYmplY3QgPSBvYmplY3QsIGZpbGVwYXRoID0gcGF0aCkKfQ&marshal=ewogICAga2VyYXM6OnNlcmlhbGl6ZV9tb2RlbChvYmplY3QpCn0&unmarshal=ewogICAga2VyYXM6OnVuc2VyaWFsaXplX21vZGVsKG9iamVjdCkKfQ&convert=&copy=&repository= 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
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
# And the following is equivalent to the current superseded
# tar_target(name, torch::torch_tensor(seq_len(4)), format = "torch"),
# except this version has a `convert` argument to handle
# cases when `NULL` is returned (e.g. if the target errors out
# and the `error` argument is "null" in tar_target()
# or tar_option_set())
tar_target(
  name = torch_target,
  command = torch::torch_tensor(),
  format = tar_format(
    read = function(path) {
      torch::torch_load(path)
    },
    write = function(object, path) {
      torch::torch_save(obj = object, path = path)
    },
    marshal = function(object) {
      con <- rawConnection(raw(), open = "wr")
      on.exit(close(con))
      torch::torch_save(object, con)
      rawConnectionValue(con)
    },
    unmarshal = function(object) {
      con <- rawConnection(object, open = "r")
      on.exit(close(con))
      torch::torch_load(con)
    }
  )
)
#> <tar_stem> 
#>   name: torch_target 
#>   description:  
#>   command:
#>     torch::torch_tensor() 
#>   format: format_custom&read=ewogICAgdG9yY2g6OnRvcmNoX2xvYWQocGF0aCkKfQ&write=ewogICAgdG9yY2g6OnRvcmNoX3NhdmUob2JqID0gb2JqZWN0LCBwYXRoID0gcGF0aCkKfQ&marshal=ewogICAgY29uIDwtIHJhd0Nvbm5lY3Rpb24ocmF3KCksIG9wZW4gPSAid3IiKQogICAgb24uZXhpdChjbG9zZShjb24pKQogICAgdG9yY2g6OnRvcmNoX3NhdmUob2JqZWN0LCBjb24pCiAgICByYXdDb25uZWN0aW9uVmFsdWUoY29uKQp9&unmarshal=ewogICAgY29uIDwtIHJhd0Nvbm5lY3Rpb24ob2JqZWN0LCBvcGVuID0gInIiKQogICAgb24uZXhpdChjbG9zZShjb24pKQogICAgdG9yY2g6OnRvcmNoX2xvYWQoY29uKQp9&convert=&copy=&repository= 
#>   repository: local 
#>   iteration method: vector 
#>   error mode: stop 
#>   memory mode: auto 
#>   storage mode: worker 
#>   retrieval mode: auto 
#>   deployment mode: worker 
#>   priority: 0 
#>   resources:
#>     list() 
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
