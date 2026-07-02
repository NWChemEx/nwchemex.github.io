---
title: "SimDE Architecture"
layout: single
permalink: /author/architecture/simde/
toc: true
toc_sticky: true
---

SimDE encapsulates the infrastructure required to:
- interact with the hardware,
- manage and run modules,
- model and express commonly occurring computational chemistry concepts, and
- define the APIs used to compute properties

SimDE is intended to have APIs that are long-term stable. This is important for
ensuring modules remain interoperable with the framework for a long time. The
goal is to get SimDE to a point where most developers will only interact with
SimDE, and will not need to perform development on it. The main exception being
the addition of standardized APIs for new properties.

Conceptually you can think of SimDE as being akin to a typical smartphone
operating system, but targeting computational chemistry. Like a smartphone OS,
SimDE manages a bunch of apps (which we call modules), takes care of inter-app
communication (passing data among the modules), and automates more mundane tasks
(like logging and saving results). SimDE is extensible in that new modules and
communication protocols can be added downstream from it, even at runtime.
Specifically the design of SimDE is such that downstream developers don't need
to modify any source code of SimDE to extend it.

This generality and flexibility comes at the cost of complexity. While design
efforts have striven to make SimDE as simple as possible, the reality is that it
is still quite verbose from a typical electronic structure user's perspective.
It is the responsibility of whatever sits on top of SimDE to provide more
user-friendly APIs (in addition to the module functionalities). More complicated
workflows can always directly access SimDE for finer-grained control.

The main components of SimDE are summarized in the following table:

| Repository | Description |
| --- | --- |
| Utilities | General classes/functions, i.e. our own personal Boost |
| ParallelZone | The runtime abstraction layer |
| PluginPlay | Framework for working with plugins |
| Chemist | Chemistry specific classes, used to define APIs |
| SimDE | Definitions of APIs, top-level repo for SimDE |


# Why Do We Need SimDE?

In why_do_we_need_pluginplay we explained the decision to build NWChemEx
on top of a modular framework. The actual design of SimDE goes beyond just using
modules, to decoupling the APIs of those modules from the guts of the framework.
This section explains why we wanted to keep the framework chemistry agnostic.

PluginPlay is designed to be a relatively generic framework. There is no
chemistry concepts tied to PluginPlay. In theory, this allows PluginPlay to be
used by other projects which are not chemistry specific. In practice, it is
probably somewhat unlikely that another project will pick up PluginPlay. Our
decision to separate the APIs from the framework is primarily based off the
realization that there needs to be a way to extend the types of modules that the
framework can use, without changing the framework's source code. This is where
the remainder of the SimDE comes in.

SimDE defines the module APIs we use throughout NWChemEx. These APIs have two
parts: the classes representing the chemistry concepts (primarily stored in
Chemist) and the actual property types. Barring the emergence of community
standards, NWChemEx will continue to use the APIs and classes within SimDE
as our standard. However, our standards do not cover every property that may be
of interest to a computational chemistry simulation (for example we are very
focused on ab initio methods and have not attempted to standardize molecular
mechanics properties). Thus we need a mechanism for developers to extend the
set of properties which can be computed. The property type system allows other
developers to define whatever APIs they like (using whatever classes they like
as well) and have PluginPlay be capable of using the resulting modules, all
without having to modify PluginPlay's source.