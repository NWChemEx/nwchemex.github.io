---
title: "How to install NWChemEx"
layout: single
permalink: /community/install/
toc: true
---

NWChemEx is a modular software package that can be extended with plugins. Here
we are focused on the user install, which will provide you with the core 
[NWChemEx package](https://github.com/NWChemEx/NWChemEx). With such an install
you can load and use additional plugins. If you are interested in developing
your own plugins/modules see the developer install 
[instructions](/developer/coding/prelminaries/).

# Install NWChemEx from a package manager

> **TODO:** Write me!

# Build NWChemEx from source

At present we only officially support building NWChemEx on Linux, or Mac. We
know that several developers have also built it on Windows using the 
[Windows Linux Subsystem](https://learn.microsoft.com/en-us/windows/wsl/about);
however, we do not officially support this.

This tutorial assumes familiarity with the terminal.
{: .notice}

## Step 1: Obtain build tools and dependencies

NWChemEx can build most of its dependencies automatically. The ones it cannot
build are commonly available packages. If you need help installing dependencies
visit [Installing NWChemEx dependencies](/community/dependencies).

## Step 2: Obtain the source code

Navigate to the directory where you want to download the NWChemEx source code,
for example:

```terminal
cd /Users/my_username/
```
will cause the next command to download the source code into the directory 
`/Users/my_username/NWChemEx` (the next command will automatically create the
`NWChemEx` subdirectory).

Clone the NWChemEx GitHub repository:

```terminal
git clone https://github.com/NWChemEx/NWChemEx.git
```

## Step 3: (Recommended) Install with pip

We advocate for the use of Python virtual environments to avoid contaminating
your system Python installation. The following snippet will create a virtual
environment in the newly created `NWChemEx` directory and then install NWChemEx
to the virtual environment.

```terminal
cd NWChemEx
python -m venv .venv
source .venv/bin/activate
pip install .
```
