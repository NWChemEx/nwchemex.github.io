---
title: "Writing Unit Tests for NWChemEx"
layout: single
permalink: /author/testing/unit/
toc: true
toc_sticky: true
---

# Writing Unit Tests for NWChemEx

Within the first party NWChemEx libraries, we aim for extensive unit testing to
ensure functionality and correctness. All classes, functions, and modules added
to any of the first party libraries will be expected to have corresponding unit
tests. Testing of functions (as well as Plugin modules) should minimally ensure
that all return routes and errors are checked. Tests for classes should do the
same for all member functions, while additionally testing that the state of all
instances is consistent at construction and after modifications. Generally, the
unit tests should be able to run quickly, and use simplified data with the
minimum level of complexity need to ensure completeness in the testing.

The C++ unit tests use the [Catch2 framework](https://github.com/catchorg/Catch2),
while python tests use the [unittest framework](https://docs.python.org/3/library/unittest.html).
Assume the following class and related comparison function are intended to be
added to one of the first party libraries:

An example unit test for the above looks like:
