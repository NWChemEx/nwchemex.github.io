---
title: "Non-Virtual Interface (NVI) Idiom"
layout: single
permalink: /author/idioms/nvi/
toc: true
toc_sticky: true
---

## What is the NVI Idiom?

In the non-virtual interface (NVI) idiom, the public interface of a
polymorphic base class is made up of **non-virtual** member functions. Each
public member function implements its behavior by calling a
**non-public virtual** member function, which derived classes override. In
NWChemEx code the virtual "hook" has the same name as the public function plus
a trailing underscore. For example, `ModuleBase::run` calls
`ModuleBase::run_`.

NVI is a special case of the Template Method design pattern
([Gamma *et al.*, 1994](#references)). The base class's non-virtual function
is the template method, and the virtual function is the step that derived
classes customize.

## Why Use the NVI Idiom?

A public virtual function does two jobs at once. It is part of the interface
that callers use, and it is the customization point that derived classes
override. NVI separates these jobs
([Sutter, 2001](#references)). This gives the following benefits:

- **The base class keeps control.** Code that must run on every call
  (checking preconditions and postconditions, normalizing arguments, logging,
  timing, caching, locking) is written once in the public function. Derived
  classes cannot skip it by accident.
- **The interface and the customization point can change separately.** The
  public signature can gain convenience overloads or default arguments
  without breaking derived classes. The hook's signature can also change
  without changing the public API.
- **Derived classes have less to implement.** They write only the part that
  actually varies.
- **The interface is consistent across language bindings.** Callers always
  use the same public function, whether the derived class was written in C++
  or Python.

The cost is one extra (usually inlined) function call, plus a second name for
each customizable operation.

## Example

### C++

```cpp
#include <stdexcept>
#include <string>

class Plugin {
public:
    virtual ~Plugin() = default;

    /// Public, non-virtual: every caller goes through here
    std::string name() const {
        auto rv = name_();
        if(rv.empty()) throw std::runtime_error("Plugin names can't be empty");
        return rv;
    }

protected:
    /// Customization point: derived classes override this
    virtual std::string name_() const = 0;
};

class SCFPlugin : public Plugin {
protected:
    std::string name_() const override { return "scf"; }
};

// Usage: SCFPlugin p; p.name();  // calls Plugin::name -> SCFPlugin::name_
```

The hook is `protected` in this example (and in PluginPlay) because
pybind11 trampoline classes must be able to override it (see below). If
Python bindings are not a concern, making the hook `private` is stricter.
Derived classes can still override a private virtual function, but they cannot
call it.

### Python

Python has no access control, so NVI in Python is a naming convention. The
public method is the interface, and the method with the trailing underscore is
the hook that subclasses override.

```python
class Plugin:
    def name(self):
        rv = self.name_()
        if not rv:
            raise RuntimeError("Plugin names can't be empty")
        return rv

    def name_(self):
        raise NotImplementedError


class SCFPlugin(Plugin):
    def name_(self):
        return "scf"
```

### Python Overriding a C++ Hook

When a C++ class that uses NVI is exposed with pybind11, a "trampoline" class
forwards the virtual hook to Python. Python subclasses then override the hook
under its C++ name. PluginPlay modules work this way: the trampoline in
`PluginPlay/cxx/src/pluginplay/module/py_module_base.hpp` uses
`PYBIND11_OVERRIDE_PURE` for `run_`. Python modules define `run_` and are
called through the non-virtual `run`.

```cpp
class PyPlugin : public Plugin {
public:
    std::string name_() const override {
        PYBIND11_OVERRIDE_PURE(std::string, Plugin, name_);
    }
};
```

```python
class MyPlugin(pluginplay.Plugin):
    def __init__(self):
        pluginplay.Plugin.__init__(self)

    def name_(self):
        return "my_plugin"
```

## References

- H. Sutter, "Virtuality," *C/C++ Users Journal* **19**(9) (2001). Available
  at <http://www.gotw.ca/publications/mill18.htm>.
- H. Sutter and A. Alexandrescu, *C++ Coding Standards: 101 Rules,
  Guidelines, and Best Practices*, Addison-Wesley (2004), Item 39: "Consider
  making virtual functions nonpublic, and public functions nonvirtual."
- E. Gamma, R. Helm, R. Johnson, and J. Vlissides, *Design Patterns: Elements
  of Reusable Object-Oriented Software*, Addison-Wesley (1994), "Template
  Method."
