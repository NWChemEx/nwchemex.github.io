---
title: "CI/CD Overview"
layout: single
permalink: /author/devops/cicd/
toc: true
toc_sticky: true
---

# CI/CD Overview

This page summarizes the key steps of the CI/CD workflows that run when a pull
request is opened against `master` and when changes are merged into `master`.
For a deeper dive into how these workflows are implemented, see the
[Continuous Deployment](/author/continuous_deployment/) section.

## Workflow Summary

![Key steps of the pull request and merge workflows.](assets/cicd_workflow.excalidraw.png)

## Pull Request Workflow (`pull_request.yaml`)

Triggered whenever a PR is opened (or updated) against `master`:

1. A developer opens a PR targeting `master`.
2. GitHub Actions runs the reusable workflows defined in `NWChemEx/.github`:
   - `check_formatting.yaml` -- runs `pre-commit` to check license headers and
     formatting.
   - `test_nwx_docs.yaml` -- verifies the repo's documentation builds
     (Doxygen and/or Sphinx).
   - `test_nwx_cmake_build.yaml` -- pulls the shared dev matrix (OS x
     compiler) from `platform_matrix.yaml`, then, for each leg, configures
     and builds the repo with CMake and runs its CTest (C++) and, optionally,
     pytest (Python) suites.
   - `test_nwx_pip_build.yaml` -- runs across the same dev matrix, but
     verifies the repo's actual `pip install` path (via `pyproject.toml`,
     e.g. scikit-build-core for the CMake-backed repos) works end-to-end,
     then runs the pytest suite against that editable install. Most repos
     with a `pyproject.toml` run this alongside `test_nwx_cmake_build.yaml`
     as a complementary check, since the two exercise different install
     paths; a pure-Python repo with no `CMakeLists.txt` passes
     `run_cmake_tests: "false"`.
3. Once all checks pass and the PR has been reviewed and approved, it is
   merged into `master`.

## Merge Workflow (`merge.yaml`)

Triggered whenever a commit lands on `master` (i.e., right after a PR merges):

1. The commit lands on `master`.
2. GitHub Actions runs the reusable workflows defined in `NWChemEx/.github`:
   - `tag.yaml` -- bumps and pushes the version tag for the new commit.
   - `deploy_nwx_docs.yaml` -- builds and publishes the repo's documentation
     to GitHub Pages.
   - `platform_matrix.yaml` -- defines the release matrix (OS x
     `cibw_build`) used for packaging.
3. `build_pypi_dist` runs as a matrix job, one leg per entry in the release
   matrix (depends on `tag-commit` and `platform_matrix`). Each leg builds a
   source distribution and/or a wheel (via `cibuildwheel` for compiled
   packages), then verifies the wheel installs cleanly, and uploads it as a
   build artifact.
4. `deploy_to_pypi` downloads the artifacts from every leg and publishes them
   using PyPI's trusted publishing (`id-token: write`, no stored API token).
5. The result is a tagged release, with documentation live on GitHub Pages
   and the package published to PyPI.

The CMake and pip build/test jobs run on bare runners using a native
(apt on Linux, Homebrew on macOS) compiler toolchain set up per matrix leg;
the docs build/deploy jobs run in a container built from
`ghcr.io/nwchemex/nwx_buildenv:latest`, which pre-installs the compilers,
math libraries, MPI, and other dependencies shared across the NWChemEx
stack. The PyPI packaging jobs also run on bare runners (plus, for compiled
packages, `cibuildwheel`'s own manylinux/macOS build containers) since they
need to produce portable wheels rather than use the ambient dev environment.

A `nightly.yaml` workflow (scheduled, not tied to PRs or merges) also exists
in each repo but is not covered here.

## Dependency Resolution: Two Independent Paths

A repo's sibling dependencies are resolved two different ways, on two
different axes, and it's easy to assume the whole graph is version-pinned
when it isn't -- worth stating plainly:

- **Python side -- pinned floors from PyPI.** Each repo's `pyproject.toml`
  lists its siblings as `>=` floors (e.g. `nwchemex-simde>=0.0.77`),
  resolved against whatever's actually published on PyPI at install time.
  `tag.yaml` bumps a patch version on every merge, so a floor rather than an
  exact pin avoids needing a bump PR in every consumer on every release.
- **C++ side -- a rolling `master` build, not version-pinned.**
  `nwxcmake/cmake/dependencies/<dep>.cmake` fetches each of the 11 NWChemEx
  C++ libraries (`chemcache`, `chemist`, `integrals`, `nux`, `nwchemex`,
  `parallelzone`, `pluginplay`, `simde`, `tensorwrapper`, `utilities`, `wtf`)
  via `FetchContent` at `GIT_TAG master` -- always the latest commit, not a
  release tag. This is deliberate: none of these 11 have a `find_package`
  fallback (unlike third-party dependencies such as `boost`, `gauxc`,
  `gau2grid`, and `libxc`, which do), so they resolve purely by `GIT_TAG`,
  including under `SKBUILD` (i.e. even a `pip install` of a compiled
  package's sdist pulls its C++ dependencies this way).

**These two paths are independent of each other.** A package's C++ sources
come from `master` at build time, while its Python floor resolves whatever
wheel is currently published -- so rebuilding the same sdist at a later date
can compile against newer C++ than the version implied by the Python floor
that originally pulled it in. This is the accepted trade for keeping the
C++ side a rolling integration build rather than version-pinned; don't
assume pinning one side pins the other.

One consequence worth knowing: `nwx_set_version`'s `get_version_from_git`
reports the latest *tag* reachable from the current checkout, which for a
branch checkout (i.e. what `FetchContent` actually gets at `master`) is the
last tagged commit, not necessarily the commit actually checked out. A
`FetchContent`'d dependency's reported CMake version is therefore
approximate, not exact -- a known, accepted limitation, not a bug to chase.
