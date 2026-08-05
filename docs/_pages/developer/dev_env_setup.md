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

where the `toolchain.cmake` file is explained below and is only needed if you
want additional control over the build system. 

## Toolchain file

The Python build is a thin wrapper over a CMake build. To pass options to the
underlying CMake build it is helpful to put the options in a toolchain file.
By convention this is a file named `toolchain.cmake` and its contents are
simply a series of CMake `set` commands like:

```cmake
set(CMAKE_CXX_COMPILER /path/to/your/C++/compiler)
# This is a comment, more set commands can follow this one.
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