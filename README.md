# rustworkx

[![License](https://img.shields.io/github/license/Qiskit/rustworkx.svg?style=popout-square)](https://opensource.org/licenses/Apache-2.0)
![Build Status](https://github.com/Qiskit/rustworkx/actions/workflows/main.yml/badge.svg?branch=main)
[![Build Status](https://img.shields.io/travis/com/Qiskit/rustworkx/main.svg?style=popout-square)](https://travis-ci.com/Qiskit/rustworkx)
[![](https://img.shields.io/github/release/Qiskit/rustworkx.svg?style=popout-square)](https://github.com/Qiskit/rustworkx/releases)
[![](https://img.shields.io/pypi/dm/rustworkx.svg?style=popout-square)](https://pypi.org/project/rustworkx/)
[![Coverage Status](https://coveralls.io/repos/github/Qiskit/rustworkx/badge.svg?branch=main)](https://coveralls.io/github/Qiskit/rustworkx?branch=main)
[![Minimum rustc 1.48.0](https://img.shields.io/badge/rustc-1.48.0+-blue.svg)](https://rust-lang.github.io/rfcs/2495-min-rust-version.html)
[![arXiv](https://img.shields.io/badge/arXiv-2110.15221-b31b1b.svg)](https://arxiv.org/abs/2110.15221)

  - You can see the full rendered docs at:
    <https://qiskit.org/documentation/rustworkx/dev>

|:warning:| The retworkx project has been renamed to **rustworkx**. The use of the
retworkx package will still work for the time being but starting in the 1.0.0
release retworkx will no longer be supported

rustworkx is a general purpose graph library for Python written in Rust to
take advantage of the performance and safety that Rust provides. It is
designed to provide a high performance general purpose graph library for
any Python application.

## Project history

Rustworkx was originally called retworkx and was was created initially to be
a replacement for [qiskit](https://qiskit.org/)'s previous (and current)
networkx usage (hence the original name).  The project was originally started
to build a faster directed graph to use as the underlying data structure for
the DAG at the center of
[qiskit-terra](https://github.com/Qiskit/qiskit-terra/)'s transpiler. However,
since it's initial introduction the project has grown substantially and now
covers all applications that need to work with graphs which includes
Qiskit.

## Installing rustworkx

rustworkx is published on pypi so on x86\_64, i686, ppc64le, s390x, and
aarch64 Linux systems, x86\_64 on Mac OSX, and 32 and 64 bit Windows
installing is as simple as running:

```bash
pip install rustworkx
```

This will install a precompiled version of rustworkx into your python
environment.

### Installing on a platform without precompiled binaries

If there are no precompiled binaries published for your system you'll have to
build the package from source. However, to be able able to build the package
from the published source package you need to have Rust >= 1.48.0 installed (and
also [cargo](https://doc.rust-lang.org/cargo/) which is normally included with
rust) You can use [rustup](https://rustup.rs/) (a cross platform installer for
rust) to make this simpler, or rely on
[other installation methods](https://forge.rust-lang.org/infra/other-installation-methods.html).
A source package is also published on pypi, so you still can also run the above
`pip` command to install it. Once you have rust properly installed, running:

```bash
pip install rustworkx
```

will build rustworkx for your local system from the source package and install
it just as it would if there was a prebuilt binary available.

Note: To build from source you will need to ensure you have pip >=19.0.0
installed, which supports PEP-517, or that you have manually installed
`setuptools-rust` prior to running `pip install rustworkx`. If you recieve an
error about `setuptools-rust` not being found you should upgrade pip with
`pip install -U pip` or manually install `setuptools-rust` with
`pip install setuptools-rust` and try again.

### Optional dependencies

If you're planning to use the `rustworkx.visualization` module you will need to
install optional dependencies to use the functions. The matplotlib based drawer
function `rustworkx.visualization.mpl_draw` requires that the
[matplotlib](https://matplotlib.org/) library is installed. This can be
installed with `pip install matplotlib` or when you're installing rustworkx with
`pip install 'rustworkx[mpl]'`. If you're going to use the graphviz based drawer
function `rustworkx.visualization.graphviz_drawer` first you will need to install
graphviz, instructions for this can be found here:
https://graphviz.org/download/#executable-packages. Then you
will need to install the [pillow](https://python-pillow.org/) Python library.
This can be done either with `pip install pillow` or when installing rustworkx
with `pip install 'rustworkx[graphviz]'`.

If you would like to install all the optional Python dependencies when you
install rustworkx you can use `pip install 'rustworkx[all]'` to do this.

## Using rustworkx

Once you have rustworkx installed you can use it by importing rustworkx.
All the functions and graph classes are off the root of the package.
For example, calculating the shortest path between A and C would be:

```python3
import rustworkx

graph = rustworkx.PyGraph()

# Each time add node is called, it returns a new node index
a = graph.add_node("A")
b = graph.add_node("B")
c = graph.add_node("C")

# add_edges_from takes tuples of node indices and weights,
# and returns edge indices
graph.add_edges_from([(a, b, 1.5), (a, c, 5.0), (b, c, 2.5)])

# Returns the path A -> B -> C
rustworkx.dijkstra_shortest_paths(graph, a, c, weight_fn=float)
```

## Building from source

The first step for building rustworkx from source is to clone it locally
with:

```bash
git clone https://github.com/Qiskit/rustworkx.git
```

rustworkx uses [PyO3](https://github.com/pyo3/pyo3) and
[setuptools-rust](https://github.com/PyO3/setuptools-rust) to build the
python interface, which enables using standard python tooling to work. So,
assuming you have rust installed, you can easily install rustworkx into your
python environment using `pip`. Once you have a local clone of the repo, change
your current working directory to the root of the repo. Then, you can install
rustworkx into your python env with:

```bash
pip install .
```

Assuming your current working directory is still the root of the repo.
Otherwise you can run:

```bash
pip install $PATH_TO_REPO_ROOT
```

which will install it the same way. Then rustworkx is installed in your
local python environment. There are 2 things to note when doing this
though, first if you try to run python from the repo root using this
method it will not work as you expect. There is a name conflict in the
repo root because of the local python package shim used in building the
package. Simply run your python scripts or programs using rustworkx
outside of the repo root. The second issue is that any local changes you
make to the rust code will not be reflected live in your python environment,
you'll need to recompile rustworkx by rerunning `pip install` to have any
changes reflected in your python environment.

### Develop Mode

If you'd like to build rustworkx in debug mode and use an interactive debugger
while working on a change you can use `python setup.py develop` to build
and install rustworkx in develop mode. This will build rustworkx without
optimizations and include debuginfo which can be handy for debugging. Do note
that installing rustworkx this way will be significantly slower then using
`pip install` and should only be used for debugging/development.

It's worth noting that `pip install -e` does not work, as it will link the python
packaging shim to your python environment but not build the rustworkx binary. If
you want to build rustworkx in debug mode you have to use
`python setup.py develop`.

## Authors and Citation

rustworkx is the work of [many people](https://github.com/Qiskit/rustworkx/graphs/contributors) who contribute
to the project at different levels. If you use rustworkx in your research, please cite our
[paper](https://arxiv.org/abs/2110.15221) as per the included [BibTeX file](CITATION.bib).

## Community

Besides Github interactions (such as opening issues) there are two locations
available to talk to other rustworkx users and developers. The first is a
public Slack channel in the Qiskit workspace,
[#rustworkx](https://qiskit.slack.com/messages/rustworkx/). You can join the
Qiskit Slack workspace [here](http://ibm.co/joinqiskitslack). Additionally,
there is an IRC channel `#rustworkx` on the [OFTC IRC network](https://www.oftc.net/)
