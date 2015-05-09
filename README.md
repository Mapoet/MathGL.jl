<!--- This file is autogenerated to contain a toc -->

## Contents

- [Requirements ](#requirements)
- [Usage and Examples ](#usage-and-examples)
    - [The C Interface ](#the-c-interface)
    - [The Higher Level Julian Interface ](#the-higher-level-julian-interface)
    - [Abusing The Matrix Syntax: Imitating Mgl Scripts in Julia ](#abusing-the-matrix-syntax-imitating-mgl-scripts-in-julia)
- [Changes to MathGL](#changes-to-mathgl)
- [TODO](#todo)

<!-- end toc -->

# MathGL.jl
**This repository probably won't work yet**

MathGL.jl shall provide a wrapper for the scientific visualization library
[MathGL](http://mathgl.sourceforge.net) written in C++.
It relies on the C interface of MathGL, wrapping it with julia to
a (hopefully) convenient degree.

MathGL.jl is currently under construction, still lacks much of the
functions provided by MathGL and is probably not portable (only linux
tested). Plotting functions from the sections "4.14 Dual plotting" on (in the manual)
are still missing, the rest should be implemented (with only few
exceptions).

### Requirements 
Besides the code of this repository, you will need a working and up-to-date
version of libmgl in a folder that julia can find via find\_library. For
additional functions (which are *not* supported yet), like qt, glut, etc.
the corresponding libraries (e.g. libmgl-qt5) must also be installed
properly.


### Usage and Examples 
#### The C Interface 
The C functions provided by MathGL are callable via the Capi submodule of
MathGL. The Capi-module is more or less complete (some functions may be
missing, but most functionality is certainly implemented); however,
it is not comfortable to use. Documentation regarding the Capi (function
names and their description) may be found in the 
[MathGL documentation](http://mathgl.sourceforge.net/doc_en/Main.html).

An example: 
```julia
using MathGL.Capi
mgl = MathGL.Capi

gr  = mgl.create_graph(1000, 600)
dat = mgl.create_data()
mgl.data_link(dat, 0.8*sin(linspace(-4pi, 4pi, 200)), 200, 1, 1)
mgl.label(gr, 'x', "x", 0, "")
mgl.label(gr, 'y', "y", 0, "")
mgl.box(gr)
mgl.axis(gr, "xyz", "", "")
mgl.axis_grid(gr, "xyz", "G|", "")
mgl.plot(gr, dat, "", "")
mgl.write_frame(gr, "../graphs/sin_C.png", "")
```
![sin_C example](/graphs/sin_C.png?raw=true)
Note however, that the C interface does only limited type checks (so
expect segfaults when using the wrong argument types).

#### The Higher Level Julian Interface 
The type structure was (quite loosely) modeled after the C++ class
structure, the most important type being the `Graph`.  The
function names (e.g. text, surf, plot, xtics, ...) were -- whenever
possible -- chosen to be the corresponding commands of the mgl scripting
language (see mathgl documentation for the details). This was done for
several reasons:
    
* The names of the mgl commands are very julian (e.g. few underscores,
  heavily overloaded)
* The C interface is tedious, the C++ interface does not really
  fit well
* An abuse of julia's matrix construction syntax makes it possible to
  write mgl script code directly in julia (with some slight
  alterations, see below).

There are, however, important functions that are not covered by the
scripting language (which is designed to handle one graph only).
Some of these deviations of function names can be found in the file
[changes.md](/changes.md), the most important ones are given
[below](#changes-to-mathgl).

The same examples as above:
```julia
using MathGL
mgl = MathGL

gr = mgl.Graph(1000, 600)
mgl.xlabel(gr, "x")
mgl.ylabel(gr, "y")
mgl.box(gr)
mgl.axis(gr, "xyz")
mgl.grid(gr, "xyz", stl="G|")
mgl.plot(gr, 0.8*sin(linspace(-4pi, 4pi, 200)))
mgl.writeframe(gr, "../graphs/sin_julia.svg")
```
#### Abusing The Matrix Syntax: Imitating Mgl Scripts in Julia 
To make it short: The following two code samples are perfectly equivalent.
*Coming soon*

## Changes to MathGL

### TODO

Still too much missing to bother enumerating...

