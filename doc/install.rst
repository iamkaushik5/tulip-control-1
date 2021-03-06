Installation
------------

The latest release of TuLiP can be downloaded from `SourceForge
<http://sourceforge.net/projects/tulip-control/files/>`_.

TuLiP is designed to work with Python version 2.7, though it should also support
Python version 3.2+.  The following additional Python packages are required to
use the core functionality of TuLiP:

* `NumPy <http://numpy.org/>`_
* `SciPy <http://www.scipy.org/>`_
* `NetworkX <http://networkx.lanl.gov/>`_
* `PLY <http://www.dabeaz.com/ply/>`_

Newcomers to scientific computing with Python should read
:ref:`newbie-scipy-sec-label`.

The default synthesis tool for GR(1) specifications is `gr1c
<http://scottman.net/2012/gr1c>`_. Please install at least version 0.9.0 (the
current release at time of writing).

The following are optional Python packages, listed with a summary of dependent
features:

* `Matplotlib <http://matplotlib.org/>`_ -- many visualization features

* `pydot <http://code.google.com/p/pydot/>`_ -- graph image file and `Graphviz
  dot <http://www.graphviz.org/>`_ export routines

* `Graphviz <http://www.graphviz.org/>`_ -- generation of images (e.g., PNG
  files) from dot code

* `polytope <https://pypi.python.org/pypi/polytope>`_ -- computations on and
  plotting of convex polytopes

* `CVXOPT <http://cvxopt.org/>`_ -- construction and manipulation of discrete
  abstractions

For computing discrete abstractions from hybrid system descriptions, it is
highly recommended---but not required---that you install `GLPK
<http://www.gnu.org/s/glpk/>`_ (a fast linear programming solver). Note that you
need to install GLPK *before* installing CVXOPT and follow the instructions in
CVXOPT installation to ensure it recognizes GLPK as a solver. If you are a
`MacPorts <http://www.macports.org/>`_ user, please note that MacPorts does not
do this linking automatically.

In previous versions of TuLiP, ``polytope`` was a subpackage of ``tulip``.  It
is now a separate package, but for convenience, a copy is bundled with some
releases of TuLiP.

Once all of the above preparations are completed, you can install TuLiP::

  $ pip install .

To enforce dependencies that are required for using parts of TuLiP intended for
hybrid systems, use ``pip install .[hybrid]``. (Your shell may try to parse
``[`` and ``]``, causing the command to fail. If so, try ``pip install '.[hybrid]'``.)

TuLiP may instead be installed `from PyPI <https://pypi.python.org/pypi/tulip>`_::

  $ pip install tulip

or, analogous to the above, ``pip install tulip[hybrid]``.

The above commands include checking of dependencies and automatic installation
of missing Python packages. (N.B., not all dependencies are Python packages.) To
only check for required and optional dependencies, but not install TuLiP, use ::

  $ python setup.py dry-check


.. _synt-tools-sec-label:

Alternative discrete synthesis tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While gr1c is required, as described in :doc:`install`, TuLiP can use other
tools for formal synthesis. Those for which an interface is available are listed
below. Also consult :doc:`specifications` concerning relevant syntax and
summaries of the specification languages. Generally, direct interfaces are
defined as modules in the subpackage ``tulip.interfaces``. However, these tools
can be accessed indirectly by appropriately setting parameters for various
functions in TuLiP, such as ``tulip.synth.synthesize()``.

These are *optional dependencies*. TuLiP is useful without having them
installed, but certain functionality is only available when they are.

GR(1)
`````

* `slugs <https://github.com/LTLMoP/slugs>`_

* a JTLV-based solver that has historically been included as part of the TuLiP
  package itself. It was originally implemented by Yaniv Sa'ar `[BJPPS12]
  <bibliography.html#bjpps12>`_.  To use it, you must have Java version 1.6 (or
  later) installed.

LTL
```

* `Lily <http://www.iaik.tugraz.at/content/research/design_verification/lily/>`_,
  which is based on `Wring <http://vlsi.colorado.edu/~rbloem/wring.html>`_.


.. _newbie-scipy-sec-label:

New to Python?
~~~~~~~~~~~~~~

If you don't already use Python for scientific computing, consider using
`Enthought Python Distribution (EPD) <http://enthought.com>`_ or `Enthought
Canopy <https://www.enthought.com/products/canopy/>`_. This may make the
installation process easier.  The EPD Free and Canopy Express distributions come
with Python and includes NumPy, SciPy, matplotlib. EPD Free or Canopy Express
together with networkx, cvxopt, and PLY is sufficient to run TuLiP.

Alternatives to Enthought are listed on the `SciPy installation webpage
<http://www.scipy.org/install.html>`_.  In particular, also try `Anaconda
<http://docs.continuum.io/anaconda/>`_.

EPD seems to work fine on most platforms but if you cannot get it to work, more
alternative packages for Mac OS X and Microsoft Windows are mentioned below.

Testing your installation
~~~~~~~~~~~~~~~~~~~~~~~~~

TuLiP is distributed with tests for itself that, like those for NumPy, provide a
way to check that TuLiP is behaving as expected.  To perform all tests from the
command-line, try ::

  $ ./run_tests.py

Use the flag "-h" to see driver script options.  More details about testing
TuLiP oriented for developers are provided in the :doc:`dev_guide`.

.. _troubleshoot-sec-label:

Troubleshooting
~~~~~~~~~~~~~~~

Regarding installation of numerical computing packages (NumPy, etc.),
for the love of all that is good, please run tests to verify proper
behavior!  ...unless you use a very well established install method.
Nonetheless, unit testing is always good practice.

If you think the necessary packages are installed, but are unsure how
to debug Python, then consider the following tips.  To see the python
path, execute::

  $ python -c 'import sys; print "\n".join(sys.path)'

Each path searched is listed on a new line. You can augment this list
by appending locations (separated by ":") to the environment variable
**PYTHONPATH**.  To see what it's currently set to, and add a new path
to "/home/frodo/work", use::

  $ echo $PYTHONPATH
  $ export PYTHONPATH=$PYTHONPATH:/home/frodo/work

You may need to tweak the export statement depending on your terminal
shell.  All of my examples are tested with zsh (the Z shell).

Ubuntu (or Debian) GNU/Linux
````````````````````````````

To install the python package dependencies, try::

  $ sudo apt-get install python-numpy python-scipy python-cvxopt python-networkx python-ply

Optionally packages can be obtained by appending ``python-matplotlib`` etc. to
the above command.

Mac OS X
````````

For installing SciPy, NumPy, consider trying
`Scipy Superpack for Mac OSX
<http://fonnesbeck.github.com/ScipySuperpack/>`_ by Chris Fonnesbeck.

When installing CVXOPT using MacPorts, there are some compatibility issues
that cause CVXOPT to fail to install.  The following customizations will link
numpy against Apple's implementation of LAPACK and BLAS and bypass this
issue:

* Uninstall atlas (if installed)::

  $ sudo port uninstall atlas; sudo port clean atlas

* Uninstall numpy (if installed)::

  $ sudo port uninstall numpy; sudo port clean numpy

* Install numpy without atlas::

  $ sudo port install py27-numpy -atlas

* Install cvxopt without atlas or dsdp::

  $ sudo port install py27-cvxopt -atlas -dsdp

Note that if you have packages that rely on numpy (such as scipy), you will
have to uninstall and reinstall those packages as well.

Microsoft Windows
`````````````````

For Windows users, type the above commands without "$" in the terminal. For
example, check the version of your Python by typing::

  python -V

To check whether the packages has been installed, open a new terminal and try::

  python
  import numpy
  import scipy
  import cvxopt

If an error message occurs, the package might not be visible on the current path
or may not be installed at all. When you cannot find a suitable package of
NumPy, SciPy, CVXOPT, and Matplotlib for your system, consider trying
`Unofficial Windows Binaries for Python Extension Packages
<http://www.lfd.uci.edu/~gohlke/pythonlibs/>`_ by Christoph Gohlke.

The package of gr1c for Windows still cannot be found. But without this package,
you can also run most TuLiP functions.

Installing other Python dependencies
````````````````````````````````````

The command ``pip install ...`` or ``easy_install ...`` will usually suffice. To
get `PLY <http://www.dabeaz.com/ply/>`_, try::

  $ easy_install ply

.. _venv-pydoc-sec-label:

virtualenv and pydoc
````````````````````

If you have installed TuLiP into a `virtualenv
<http://www.virtualenv.org/>`_-built environment, then the documentation may not
be visible through `pydoc <http://docs.python.org/library/pydoc.html>`_ .  We
describe two solutions here, the first being more general. ::

  $ alias pydoc='python -m pydoc'

If that fails, try to explicitly augment the path used by pydoc with an alias.
E.g., suppose your username is "frodo", you are running Python v2.6, and your
virtual environment is called "PY_scratch" under your home directory.  Then the
appropriate alias is similar to::

  $ alias pydoc='PYTHONPATH=$PYTHONPATH:/home/frodo/PY_scratch/lib/python2.6/site-packages/ pydoc'

To set this alias for every new terminal session, add the line to your shell
startup script; e.g., ``~/.bashrc`` for bash, or ``~/.zshrc`` for zsh.  To test
it, try looking at the transys subpackage by entering::

  $ pydoc tulip.transys

.. rubric:: Footnotes

.. [#f1] On Unix systems, in particular GNU/Linux and Mac OS X, the
         terminal shell treats ``~`` as a special symbol representing
         the home directory of the current user.

remote server installation
``````````````````````````

Instructions for installing ``tulip`` and its dependencies from scratch on a
Unix server can be found `here
<https://github.com/tulip-control/tulip-control/blob/master/contrib/nessainstall/instructions.md>`_.
