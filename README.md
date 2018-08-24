# multi-core-python
Enabling CPython multi-core parallelism via subinterpreters.

This repo is for tracking the effort and as a place to keep any tooling.

[wiki](https://github.com/ericsnowcurrently/multi-core-python/wiki) | [projects](https://github.com/ericsnowcurrently/multi-core-python/projects)

## Project Summary

From my [python-ideas post](https://mail.python.org/pipermail/python-ideas/2015-June/034177.html) (June 2015):

    Python's multi-core story is murky at best.  Not only can we be more
    clear on the matter, we can improve Python's support.  The result of any
    effort must make multi-core (and concurrency) support in Python obvious,
    unmistakable, and undeniable (and keep it Pythonic).

The goal of this project is to do just that, by using the existing subinterpreter C-API.

### Solution

The minimal multi-core solution will involve:

* resolving existing bugs and blockers
* exposing the existing support (from C-API) in a stdlib module
* adding a mechanism to safely "share" basic immutable objects between interpreters
* no longer sharing the GIL between interpreters

Once a minimal solution is in place we can expand from there.

At a high level, we're doing the following concrete tasks:

* [PEP 554 (Multiple Interpreters in the Stdlib)](https://www.python.org/dev/peps/pep-0554/)
* improve interpreter isolation (especially by moving fields from `PyRuntimeState` to `PyInterpreterState`)

### Constraints / Requirements

1. no significant impact on single-threaded performance
1. maintain backward compatibility (C-API, etc.)
1. (pseudo-)compatibility with multiprocessing/threading/concurrent.futures APIs
1. a multi-core concurrency model/approach that fits our brains
1. Python APIs
1. supportable on other Python implementations
