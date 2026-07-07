---
title: "Overview of NWChemEx Development"
layout: single
permalink: /developer/overview/
---

The NWChemEx ecosystem is predicated on the assumption that developers are 
interested in creating and consuming modular software. Our purpose here is
to provide enough conceptual background so that developers can understand
what modules are and how modules interact with the rest of the NWChemEx 
ecosystem. From there developers will be ready to begin learning how to write
their first module.

## What is a module?

In software engineering, modules are defined as reusable, single-purpose units 
of code that encapsulate their implementation details. The key aspects of this 
definition are:

- *Reusable.* A piece of software is reusable if it can be used from more than
  one place.
- *Single Purpose.* A module should do one task. To do multiple tasks use
  multiple modules.
- *Encapsulate.* Modules should be black-boxes, i.e., the caller of the module
  should not care how the module does what it does.

There are also several corollary properties that follow from this definition:

- *Generalization.* A module must represent some sort of generalization. By 
  contrast, if a code unit could only be used in a very specific situation it 
  would not be reusable. 
- *Discoverable.* Again follows from the reusable property. If we are to call a
  module from multiple places it must be possible for each of those places
  to find the module.
- *Composable.* Each module performs one task. Usually those tasks are much
  simpler than the user's goal. We thus must be able to use results from
  multiple modules to achieve the goal.

Ultimately the concept of a module provides a mechanism for managing the 
complexity of a problem. More specifically, modules allow us to solve new
problems by building on solved problems. For example, say we are trying to sort
a list. Since modules for comparing elements of the list likely exist we can
implement our algorithm in terms of such modules (if the modules did not exist 
we could write them, or we could hard-code the the comparison into the module
for the sorting algorithm). Similarly, if we were writing an algorithm 
for finding the set of elements common to two lists, we will benefit from the 
module for sorting a list.

## What is modular software?

At face value modular software is simply software which relies on modules. In
practice almost all modern software is modular at some level; for example, it 
probably uses functions, classes, and libraries to break the code up as
opposed to dumping everything into one giant main function. The real difference
between modular and non-modular software is what is called "cohesion." Cohesion 
is the extent to which things bundled in a module belong together. Modules where
everything "belongs together" are said to have "high-cohesion" whereas those
which only somewhat belong together are said to have "low-cohesion". Software
made of many high-cohesion modules is termed modular software whereas that
relying on many low-cohesion modules is usually monolithic.

Why does this matter? Decades of software engineering experience has shown that
it is much harder to maintain, extend, and reuse low-cohesion modules. Moreover,
software formed from low-cohesion modules tends to be more buggy, harder to
use, and harder to extend. This is because modifying low-cohesion modules is
a "game of whack-a-mole." Since low-cohesion modules bring together many
loosely related pieces, changes to those pieces can easily break "distant"
pieces of the software. By contrast, changes to high-cohesion modules tend to
only have ramifications in the module.

## Modularity within NWChemEx

The fundamental premise of the NWChemEx ecosystem is that developers want 
to develop high-performance, high-cohesion modules and that software
should be designed in a way that capitalizes on such modules. To that end it is
important to note that some modules, like those implementing 
optimization algorithms, can be used in many different circumstances. Similarly,
for many problems, there are often a number of different ways to get the same 
solution (again optimization is a good example). Continuing with the optimizer
example, one can imagine that users may have personal preferences for which 
optimizer module to use where and/or want to immediately used a bleeding-edge
module wherever optimization is needed. NWChemEx solves this by relying on
dynamic loading of modules, i.e., the decision of which modules to use where
is put off until the software is run.

The NWChemEx ecosystem is comprised of plugins and 
[SimDE](https://github.com/NWChemEx/SimDE). Plugins are libraries
of modules which can be loaded dynamically and SimDE is the software development
kit that contains the infrastructure for loading plugins as well as the 
fundamental data structures and APIs needed to interface with plugins.  

## Next Steps

To start developing you will need to install SimDE and/or NWChemEx. Strictly
speaking plugins can be created with SimDE alone; however, many developers find
it easier to develop plugins if they can test them using other plugins in the
ecosystem. See [here](/community/install/) for build instructions.