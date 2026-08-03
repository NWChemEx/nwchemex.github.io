---
title: "Installing NWChemEx dependencies"
layout: single
permalink: /community/dependencies/
toc: true
---

NWChemEx can not build the following dependencies:

- git
- C and C++ compilers
- CMake
- MPI
- BLAS/LAPACK
- Boost
- Python
- Ninja

This page will provide you strategies for obtaining them.

# Obtaining dependencies on Ubuntu

 > **TODO:** Write me!!!

# Obtaining dependencies on MacOS

We recommend using [Homebrew](https://brew.sh) for obtaining packages on Mac.

## Obtaining Homebrew

- Go to `https://brew.sh`.
- Copy the command they have listed (or follow one of the other install methods)
- Open a terminal and paste the command.
- Enter your password.
- Press enter to accept the install parameters.
- Recommended: run the commands under "Next steps" to add Homebrew to your path.

## Installing dependencies with Homebrew

With Homebrew the dependencies and corresponding terminal commands are:

- git `brew install git`.
- C and C++ compilers (MacOS comes with `clang` and `clang++`)
- CMake `brew install cmake`.
- MPI `brew install mpich`.
- Boost `brew install boost`.
- Ninja `brew install ninja`.

> **Note:**
> To find the Python developer libraries run ``python3-config --include``.
> Add the output of this command (without the ``-I``) to the CMake variables:
> ``Python_INCLUDE_DIRS`` and ``Python3_INCLUDE_DIRS``.
