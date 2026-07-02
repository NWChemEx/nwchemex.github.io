---
title: "File Structure for NWChemEx Repositories"
layout: single
permalink: /author/conventions/file_structure/
toc: true
toc_sticky: true
---

To the extent possible, we want all NWChemEx repositories to follow the same
file structure. Since each repo in the organization is ultimately different,
it will in general not be possible for every repo to be laid out exactly the
same; however, we strive to only deviate from the standard layout when
absolutely necessary.

## Why Do We Need to Standardize the Repository Layouts?

Admittedly, the repository layout adopted by all repositories in the NWChemEx
organization was chosen based on personal preference of the lead developers.
However, now that a standard has been agreed upon it is important that we
adhere to it. In particular, by standardizing the file layout of each
repo we are able to facilitate:

- Confidence. Well laid out repositories instill a more professional impression
  on anyone viewing the repository's files.
- Finding files. Users/developers familiar with the layout of one NWChemEx
  repository can quickly and easily find files in another.
- Automation. If all the repositories are laid out the same it is much easier
  for CI/CD to maintain them.
- On-boarding new developers. Where possible our layouts adhere to wider
  file system standards, standards which are familiar to many developers outside
  the NWChemEx organization. By adhering to such standard we lower the barrier
  to entry for new developers.

## File Naming Conventions

- To avoid pitfalls related to differences in operating system behavior, all
  files should be "lower_snake_case", i.e., all lowercase letters, underscores
  for separating words.
- It is strongly recommended you stick to letters, numbers, and underscores
  only. Notably avoid symbols like `*`, `?`, `(`, `)`, `'`,
  and `"`.

### Extensions

- C++ header files end with `*.hpp`, e.g., `file_name.hpp`.
- C++ source files end with `*.cpp`, e.g., `file_name.cpp`.
- C++ inline implementation files end with `*.ipp`, e.g., `file_name.ipp`.
- CMake modules end with `*.cmake`, e.g., `file_name.cmake`.
- (GitHub-flavored) Markdown end with `*.md`, e.g., `file_name.md`.
- Python source files end with `.py`, e.g., `file_name.py`.
- ReStructured Text files end with `.rst`, e.g., `file_name.py`.

### Exceptions

The following exceptions to the above file naming conventions are allowed:

- CMake build systems should be stored in one or more `CMakeLists.txt`.
  - Reason for exception: CMake convention.

## NWChemEx Repository Structure

Every repository owned by the NWChemEx organization must follow the same
structure, which is as follows:

```text
<repo-root>/
├── .github/                # GitHub configuration files
├── cmake/                  # CMake helper modules
├── docs/                   # Documentation
├── examples/               # Example codes and scripts
├── cxx/                    # Root for the C++ library
|   ├── include/            # Public C++ headers
|   └── src/                # C++ source files
├── python/                 # Root for Python source code
|   └── <repo-name>/        # Root for Python package  
|       └── __init__.py     # Python package initializer
├── tests/                  # Unit and integration tests
|   └── cxx/                # Root for C++ tests
|   └── python/             # Root for Python tests
├── CMakeLists.txt          # CMake build script
├── README.md               # Project overview
├── LICENSE                 # License information
└── pyproject.toml          # Python project configuration
```

Here:

- `<repo-name>` should be a snake_case "C"-style name, i.e., no spaces, no
  special characters, all lowercase letters, and underscores used to join
  words.

   - Motivation: Easier on the user (e.g., no needing to escape spaces) and 
     consistent regardless of whether the file system is case sensitive or 
     not.

- We opted for `cxx` instead of `cpp` as it is more consistent with how we
  were already sanitizing "C++"  (e.g., `nwx_cxx_api`).

## Why this layout?

## Considerations

- Most repositories will be C++ projects with Python bindings.
   - Python bindings will be the preferred way for users to leverage the
     functionality provided by the code.
   - Linking to the C++ library from other C++ projects should also be possible.  
- Repositories may include additional Python source code.
- Some repositories may be pure Python projects.

## Existing Examples

Here's some existing examples we consulted.

- [Demo repo for a C++/Python project](https://github.com/smrfeld/cmake_cpp_pybind11_tutorial)
   - Advocates for a `cpp/` directory for C++ code and a `python/` directory
     for Python code.
   - C++ tests are in `cpp/tests/` and similar for Python tests.

### Alternative Layouts

In coming up with the above we considered some alternative layouts. This section
explains why we rejected those alternatives.

#### The "Flat" Python Layout

A common Python layout is to do something like:

```text
<repo-root>/
└── <repo-name>/            # Root for Python package  
|   └── __init__.py         # Python package initializer
├── docs/                   # Documentation
├── tests/                  # Unit and integration tests
└── ...
```

This is called a "flat" layout because there is no separation between source
code and other files (e.g., documentation, tests, build scripts, *etc.*). The
alternative is:

```text
<repo-root>/
├── src/                 # Root for Python source code
|   └── <repo-name>/        # Root for Python package  
|       └── __init__.py     # Python package initializer
├── docs/                   # Documentation
├── tests/                  # Unit and integration tests
└── ...
```

[Src vs Flat](https://medium.com/@adityaghadge99/python-project-structure-why-the-src-layout-beats-flat-folders-and-how-to-use-my-free-template-808844d16f35)
provides a good discussion of the pros and cons of each layout. The main reason
to avoid flat is that it can lead to import issues.
