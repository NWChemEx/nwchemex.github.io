---
title: "Setting up a development environment"
layout: single
permalink: /developer/dev_env_setup
toc: true
toc_sticky: true
---

This page walks you through the basic setup of an NWChemEx development 
environment, or dev environment for short. 

# Housing the dev environment

The source code for NWChemEx is spread out over a series of repos. It is 
strongly recommended that you create a folder named something like 
`nwx_workspace` and populate `nwx_workspace` with clones of each repo you 
intend to develop (as well as any dependencies you want to build from source). 
You should not need to clone repos you will not be developing for. We'll term a 
directory like `nwx_workspace` your "NWChemEx workspace."

As a bare minimum you'll want to clone the repo where your module/code will 
live. For example if you are writing a module that will live in the SCF repo, 
you'll also need to clone `NWChemEx/SCF`. For this example your NWChemEx
workspace will look like:

```
nwx_workspace/
|-- SCF/
`-- toolchain.cmake
```

where the `toolchain.cmake` file is explained in detail in the next subsection. 

## Toolchain file

> **Note**: This section is only relevant for developing in a repository that
> contains a CMakeLists.txt. This section is relevant even if you only plan to
> work on the Python pieces of the repo.

Fundamentally, most of the repos in the NWChemEx ecosystem leverage CMake as the
build system. In these cases, the Python build is a thin wrapper over a CMake 
build. Thus, regardless of whether you are building the repo with CMake or 
Python, controlling the build requires passing options to CMake. To make your
life easier, it helps to put all of the options you want to pass into a single
file. By convention this is a file named `toolchain.cmake` and its contents are
simply a series of CMake `set` commands like:

```cmake
set(CMAKE_CXX_COMPILER /path/to/your/C++/compiler)
# This is a comment, more set commands can follow this one.
```

If you plan to build the development repo with CMake directly, you pass the full
path of the toolchain file to CMake like:

```
cmake -DCMAKE_TOOLCHAIN_FILE=/path/to/toolchain.cmake
```

# Creating an editable install

1. Build NWChemEx
2. Create an editable install 

```terminal
 pip install -e ".[dev]" \
              --config-settings=cmake.define.CMAKE_TOOLCHAIN_FILE="/path/to/toolchain"
```

You only have to include the `--config-setings=...` flag if you are providing 
a toolchain. 

To run the C++ tests:
```terminal
ctest --test-dir build
```

To run the Python tests:
```terminal
python -m pytest
```

The editable install will only automatically pickup Python changes. You will 
have to rebuild the C++ source for C++ changes to take effect. To do this you
simply run CMake again:

```terminal
cmake --build build
```

Then run the C++ and/or Python tests as appropriate.

# Next steps

It is strongly suggested you do development in an integrated development 
environment, or IDE, see 
[Using IDEs to develop NWChemEx](/developer/coding/ides) for more information.