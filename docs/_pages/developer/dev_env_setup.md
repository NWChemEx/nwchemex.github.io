---
title: "Setting up a development environment"
layout: single
permalink: /developer/dev_env_setup
toc: true
toc_sticky: true
---

This page walks you through the basic setup of an NWChemEx development 
environment, or dev environment for short. 

## Housing the dev environment

The source code for NWChemEx is spread out over a series of repos. It is 
strongly recommended that you create a folder named something like 
`nwx_workspace` and populate `nwx_workspace` with clones of each repo you 
intend to develop (as well as any dependencies you want to build from source). 
You should not need to clone repos you will not be developing for. We'll term a 
directory like `nwx_workspace` your "NWChemEx workspace."

As a bare minimum you'll want the `NWChemEx/NWChemEx` repo in your
NWChemEx workspace. More than likely you will need to clone at least one
additional repository, namely the repo where your module/code will live. For
example if you are writing a module that will live in the SCF repo, you'll
also need to clone the `NWChemEx/SCF`. For this example your NWChemEx
workspace will look like:

```
nwx_workspace/
|-- external/
|-- NWChemEx/
|-- SCF/
`-- toolchain.cmake
```

where `external/` contains any dependencies you installed from source (see
[Installing NWChemEx dependencies](/community/dependencies) for more
information) and the `toolchain.cmake` file will be described below. 

## Toolchain file

The last piece of the preliminary set-up is the toolchain file. By convention
this is a file named `toolchain.cmake`. Its contents are a series of CMake
`set` commands like:

```cmake
set(CMAKE_CXX_COMPILER /path/to/your/C++/compiler)
set(BUILD_TESTING TRUE) # Always a good idea to enable tests when developing
```

which helps you set a consistent set of CMake variables for all builds. 

For development purposes we also want to tell the build system to use our local
copies of repos we're developing in. This is done with CMake's
`FETCHCONTENT_SOURCE_DIR_XXX` variables. For example if we're developing code
for the SCF repo we need to also add:

```cmake
set(FETCHCONENT_SOURCE_DIR_SCF /path/to/nwx_workspace/SCF)
```

to our toolchain. If you are writing code for multiple repos you simply set
multiple `FETCHCONTENT_SOURCE_DIR_XXX` values, one for each repo.

> **Note:**
> Having a line like ``FETCHCONENT_SOURCE_DIR_SCF`` in a toolchain file is fine
> when building repos which live upstream from SCF (such as SimDE and Chemist)
> and even for SCF itself. Thus it's possible to use one toolchain file for the
> entire workspace.

# Creating an editable install

```terminal
pip install $(python3 -c "import tomllib; print(' '.join(r for r in tomllib.load(open('pyproject.toml','rb'))['build-system']['requires'] if 'nwxcmake' not in r))")
```

```terminal
pip -e .[dev]
```


# Next steps

It is strongly suggested you do development in an integrated development 
environment, or IDE, see 
[Using IDEs to develop NWChemEx](/developer/coding/ides) for more information.