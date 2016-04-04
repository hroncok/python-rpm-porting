.. toctree::
   :hidden:

   self
   applications
   modules
   tools


=======================================
Welcome to the Python RPM Porting Guide
=======================================

.. warning::
    This guide is in *outline* stage. The details are not yet fleshed out.

This document aims to guide you through the process of porting your Python 2 Fedora package to Python 3.

.. note::
    If you struggle with porting of your package and would like help or more information, please contact us at the `python-devel mailing list`_ (`web interface`_) and we will try to help you and/or improve the guide accordingly.

.. _python-devel mailing list: https://lists.fedoraproject.org/admin/lists/python-devel.lists.fedoraproject.org/
.. _web interface: https://lists.fedoraproject.org/archives/list/python-devel@lists.fedoraproject.org/


Is your package ready to be ported?
-----------------------------------

First thing you need to figure out is if the software you're packaging is ready to be packaged for Python 3.

Does upstream support Python 3?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Look  upstream and try to find out if the software is released with Python 3 support. First look at the front page of the project, Python compatibility is oftentimes listed there. If not, look at release notes or the changelog history. You can also look at issues and pull requests.

However, the important thing to note is that the python 3 support needs to be *released*, not just committed in the version control system (git, mercurial,...).

Are the dependencies of your package ported to Python 3 in Fedora/RHEL?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before you start porting, it's imperative that you check if all dependencies of your package are also ported to Python 3 in Fedora or RHEL.

You may encounter a situation where your software is Python 3 ready upstream, but it uses some dependencies that are packaged only for Python 2 in Fedora or RHEL. In that case try to communicate with the maintainer of the needed package and try to motivate and/or help them with porting of the package.


What type of software are you packaging?
----------------------------------------

There are three distinct types of Python packages, each with different instructions for porting, so be mindful of which you chose:

1. Applications written in Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If your package is not being imported by third-party projects (e.g. ``import some_module`` in a python file), your package is most likely an application.

However, if your application has a plugin system or interacts with user code, you should use the section for :doc:`Python tools <tools>`

*See:* :doc:`Porting applications written in Python <applications>`

2. Python modules
^^^^^^^^^^^^^^^^^

If your package is being imported by third-party projects, but does not have any executables, you're dealing with a standard Python module.

*See:* :doc:`Porting Python modules <modules>`

3. Python tools
^^^^^^^^^^^^^^^
This section is useful in the following cases:

* If your application has a plugin system or in any way interacts with user code.
* If your package is being imported by third-party projects and also has an executable.
* If your package needs to ship both Python 2 and Python 3 executable.

*See:* :doc:`Porting Python tools <tools>`