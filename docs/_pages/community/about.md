---
title: "About the NWChemEx Project"
layout: single
permalink: /community/about/
toc: true
---

> **TODO:**
> Pictures, make this not a wall of text.

TL;DR NWChemEx is a quantum chemistry software package which
strives to adhere to modern software engineering best practices. These practices
champion a separation-of-concerns approach which results in a package with many
well-defined pieces. Users benefit from the "app-store" like experience
and developers benefit by being able to quickly inject code throughout the
software for rapid prototyping, custom workflows, and performance tuning.

## NWChemEx Focuses on Quantum Chemistry

Chemistry is the study of matter, i.e., things made of atoms. The properties of 
matter stem from how the constituent atoms interact. Given an arrangement of
atoms, computational chemistry strives to accurately compute the interactions
among the atoms and therefore the resulting properties. While the 
equations for computing the atomic interactions for a set of atoms are known, 
even with today's supercomputers these equations can only be exactly solved
for systems containing a few atoms. Most systems of interest in chemistry 
contain many molecules interacting with each other and usually each of the
molecules contains somewhere on the order of 10 to 100 atoms. Exact solution
for systems of chemical interest is unlikely to be possible any time soon.

NWChemEx is targeted at providing high-performance implementations of quantum
chemistry methods. Generally speaking, quantum chemistry methods remain 
accurate by staying true to the physics of the system, but are also practical 
because they only treat a subset of interactions. While more practical than
the exact equations, most quantum chemistry methods still require a substantial
amount of computational resources. NWChemEx actively strives to increase the
applicability of quantum chemistry methods by developing new reduced cost
algorithms and by taking full advantage of today's modern hardware.

## NWChemEx Philosophy

### User Friendly

At launch NWChemEx has a limited selection of features; however, we strive to
make using those features easy for users regardless of whether the user's use 
case is a heroic exascale computation, a high-throughput campaign, or a
teaching exercise for undergraduate physical chemistry. 

For more details see [NWChemEx Community](/community/).

### Software Ecosystem Friendly
Many software developers within scientific computing are embracing modularity.
This means there is an abundance of existing, open-source, performant software
which can be leveraged in an algorithm instead of needing to "reinvent the 
wheel." To capitalize on this, NWChemEx is built on a plugin system which makes 
it easy for users to inject code ---be it an existing library, custom behavior,
or an entire prototype routine --- into existing algorithms. Moreover this can
be accomplished without needing to directly modify the existing code base.

For more details see [NWChemEx Developers](/developer/).

### Theory Friendly
At present the majority of NWChemEx's development has been on infrastructure
with the goal of making developing electronic structure theory easier. To that 
end, NWChemEx embraces object-oriented programming (OOP). Using OOP it is much 
easier to decouple the developer's intent from the optimization of that intent. 
For example, using objects meant to represent wavefunctions and operators, the 
developer can write a quantum chemistry method in a manner similar to what one 
finds in a textbook. Under the hood, the wavefunction and operator classes can
dispatch to routines which know how to efficiently form, compose, and operate
on tensors. 

For more details see [NWChemEx Authors](/author/).