---
title: "How to install NWChemEx"
layout: splash
permalink: /community/install/
toc: true
---

> **TODO:**
> These instructions are ancient and need updated after the build system
> overhaul.

NWChemEx is a modular software package that can be extended with plugins. All
of these tutorials focus on building the core NWChemEx package 
[repository](https://github.com/NWChemEx/NWChemEx). Such an install will give
you SimDE (i.e., the NWChemEx software development kit), the core plugins
maintained by the NWChemEx authors, and the user interface.
{: .notice}

# Building from source

At present we only officially support building NWChemEx on Linux, or Mac. We
know that several developers have also built it on Windows using the 
[Windows Linux Subsystem](https://learn.microsoft.com/en-us/windows/wsl/about);
however, we do not officially support this.

This tutorial assumes familiarity with the terminal.
{: .notice}

## Step 0: Obtain build tools

TODO: Can we synchronize the versions with our CI/CD pipelines? Maybe a link?
{: .notice--info}

You will need:

1. A C++ compiler supporting the C++17 standard. We test with GCC 11 and 
Clang 14. Other versions may work, but we guarantee those.
2. CMake. We require 3.17 minimally, but test with 3.22.
3. Python 3.  We test with 3.10.

## Step 1: Obtain dependencies.

The NWChemEx build system will obtain a number of dependencies for you, but not
all of them. You will need to manually obtain the following common 
dependencies (clusters likely already have them; if you don't have them check
your package manager):

1. Boost Libraries. We test with 1.74.
2. An MPI implementation supporting version 3 of the MPI standard. We test with OpenMPI 4.1.2.
3. BLAS. We test with OpenBLAS.
4. LAPACK. We test with OpenLAPACK.
5. The original NWChem (by default used as a fall back for methods not yet
   implemented in NWChemEx). The executable must be in your path.

## Step 2: Obtain the NWChemEx source repository.

We recommend building in a workspace directory. This is simply a new directory
you create to hold the files we will create during this tutorial as well as
the source code you clone. By using a workspace it is easy to clean-up after
you are done (i.e., just delete the workspace). 
{: .notice--info}

Start by creating a workspace directory and changing to it:
```bash
mkdir nwx_workspace
cd nwx_workspace
```

Now we use `git` to obtain the NWChemEx source from GitHub:

```bash
git clone https://github.com/NWChemEx/NWChemEx
```

This command will clone the NWChemEx repository into a subdirectory named 
`NWChemEx`.

## Step 3: Prepare toolchain file

CMake best practices are to create a toolchain file which contains your build
settings. By convention this file is called `toolchain.cmake`. The minimum
contents of this file are:

```cmake
set(NWX_MODULE_DIR /full/path/to/where/you/want/plugins/to/be/installed)
```

other useful CMake variables you may want to set include (note variables are
case-sensitive):

- [`CMAKE_C_COMPILER`](https://tinyurl.com/5n7m2r2b). The full path to the C 
  compiler to use.
- [`CMAKE_CXX_COMPILER`](https://tinyurl.com/5n7m2r2b). The full path to the 
  C++ compiler to use.
- [`CMAKE_PREFIX_PATH`](https://tinyurl.com/3ruvbpk5). A list of additional
  paths where CMake should look for pre-installed dependencies. 
- [`Python3_EXECUTABLE`](https://tinyurl.com/255hzccm). The full path to the
  Python interpreter to use.

## Step 4: Configure NWChemEx

You should still be in the `nwx_workspace` directory created during Step 2. With
the toolchain written it is now time to configure NWChemEx. We recommend a
variation on:

```bash
cmake -H<nwchemex_directory> \
      -B<build_directory> \
      -DCMAKE_INSTALL_PREFIX=</where/you/want/nwx/installed> \
      -DCMAKE_TOOLCHAIN_FILE=<path/to/toolchain.cmake>
```

You will need to provide values for the quantities in angle brackets. These
quantities are:

- `<nwchemx_directory>`. This is the name of the subdirectory containing the
  NWChemEx source code you cloned in Step 2. If you followed Step 2 exactly
  the value needed should be `NWChemEx`.
- `<build_directory>`. This is the name of the directory the build system will
  use as scratch during the build process. Conventionally the value is `build`,
  but you're free to use whatever value you want.
- `</where/you/want/nwx/installed>`. Because of how CMake works this variable
  can NOT be set in the toolchain file and must be specified on the command
  line. The value should be the full path to the root of the directory where
  you want NWChemEx installed.
- `</path/to/toolchain.cmake>`. This is the full path to the `toolchain.cmake`
  file. If you followed this tutorial exactly it should be the full path to
  the `nwx_workspace` directory plus `toolchain.cmake`.

This command will likely take a while (depending on your internet speed on
the order of 5 or so minutes is not uncommon).
{: .notice}

Unless you are comfortable with CMake, we strongly recommend deleting the
build directory every time you reconfigure. Particularly if the configuration
was not successful.
{: .notice--warning} 

## Step 5: Build NWChemEx

If configuration was successful, you should be able to build NWChemEx by:

```bash
cmake --build <build_directory> --parallel <jobs>
```

where `<build_directory>` is the path to the build directory you specified in
Step 4 and `<job>` is the number of cores you want to use for compiling.

## Step 6: (Optional) Run the Test Suite

Assuming you configured with the CMake option `BUILD_TESTING` set to a truthy
value, you will now be able to run the test suite via:

```bash
cmake --build <build_directory> --target test
```

If you did not set `BUILD_TESTING` to a truthy value, then this command will
not find any tests.

## Step 7: Install

Now all that is left is to install NWChemEx. This is done via:

```bash
cmake --build <build_directory> --target install
```

Depending on the value you picked for `CMAKE_INSTALL_PREFIX` you may need to
run this command with administrative privileges.