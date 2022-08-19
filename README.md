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

### Status

* slow progress
* high level of community excitement
* increasing number of collaborators
* [phase 1](https://github.com/ericsnowcurrently/multi-core-python/projects/4) ("minimal solution") completion getting closer
   * still hopeful for Python 3.9
   * PEP 554 low-level implementation mostly complete
   * currently working on moving globals to runtime/interpreter state (or removing them)
      * will take a while (started with ~1500 global variables)
   * next up is making memory allocators per-interpreter (and then the GIL itself)

### Key Collaborators

This project wouldn't be what it is without the help of these great people:

* @ncoghlan - inspired the solution; PEP 432
* @encukou - better subinterpreter support in extension modules
* @vstinner - all sorts of runtime stuff
* @emilyemorehouse - reviews & sanity checks
* @nanjekyejoannah - C-API; PEP 554 high-level module
* @eduardo-elizondo (Facebook/Instagram) - extension modules; globals
* @vsajip - globals
* many others

## Contributing

There are many ways to contribute to this project.  Aside from the main technical work in the CPython runtime, there are also a number of jobs that need to be done that are less expert-related and even (somewhat) non-technical.  All of it is important and anyone interested in helping is welcome!

Keep in mind that this project/repo is actually just a tool to organize the effort.  The actual work is done on [bugs.python.org](https://bugs.python.org/), [github.com/python/cpython](https://github.com/python/cpython/), and the [python-dev](https://mail.python.org/archives/list/python-dev@python.org/) mailing list.

Also note that contacting [@ericsnowcurrently](https://github.com/ericsnowcurrently) directly is fine, but you might get a faster response through those other channels or through the issue tracker here. :)

For more information see the [wiki page](https://github.com/ericsnowcurrently/multi-core-python/wiki/9-%22How-Can-I-Help%3F%22).
