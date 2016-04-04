Porting applications written in Python
======================================

This section is for packages that are not being imported by third-party projects (e.g. ``import some_module`` in a Python file) and that do not interact with user code (such as plugins).

If this is not the case for your software, look into :doc:`other sections <index>`.


Porting the specfile to Python 3
--------------------------------

Applications behave the same when run under Python 2 and Python 3, therefore all you need to do is change the spec file to use Python 3 instead of Python 2.

**In essense, porting of an application RPM mostly consists of going through the spec file and adding number 3 or substituting number 3 for number 2 where appropriate. Occasionally also substituting old macros for new ones that are more versatile.**

So let's take an example spec file and port it to illustrate the process. We start with a spec file for an application that is being run with Python 2:

.. literalinclude:: diffs/application.spec.orig
   :language: spec
   :caption: Example spec file for an application running on Python 2.

.. _modifications:

Modifications
-------------

First it is recommended to update the software to the newest upstream version. If it already is at the latest version, increment the release number.


BuildRequires and Requires
^^^^^^^^^^^^^^^^^^^^^^^^^^

Change ``BuildRequires`` from ``python2-devel`` to ``python3-devel`` and adjust all other ``Requires`` and ``BuildRequires`` entries to point only to python3 packages.

**It is very important that you don't use any Python 2 dependencies as that would make your package depend both on Python version 2 and version 3, which would render your porting efforts useless.**


.. _build-section:

%build
^^^^^^

In the build section you can find a variety of macros, for example ``%py_build`` and its newer version ``%py2_build``. You can freely substitute these by the new Python 3 macro ``%py3_build``.

Occasionally you will find a custom build command prefixed by the ``%{__python}`` or ``%{__python2}`` macros, or in some cases just prefixed by the python interpreter invoked without a macro at all, e.g.::

    %{__python} setup.py build
        or
    python setup.py build

In these cases first try substituting the whole build command by the new smart macro ``%py3_build`` which should in many cases correctly figure out what ought to be done automatically. Otherwise change the invocation of the python interpreter to the ``%{__python3}`` macro.


%install and %check
^^^^^^^^^^^^^^^^^^^

The ``%install`` section will oftentimes contain the ``%py_install`` and ``%py2_install`` macros; you can replace these with the new Python 3 macro ``%py3_install``.

Furthermore, as in the preceding :ref:`build-section` section, you will frequently find custom scripts or commands prefixed by either the ``%{__python}`` or ``%{__python2}`` macros or simply preceded by an invocation of the python interpreter without the use of macros at all.

In the install section, try substituting it with the new ``%py3_install`` macro, which should figure out what to do automatically. If that doesn't work, or if you're porting the ``%check`` section, just make sure that any custom scripts or commands are invoked by the new ``%{__python3}`` macro.


%files
^^^^^^

In the files section you can regularly find the following macros: ``%{python2_sitelib}``, ``%{python2_sitearch}``, ``%{python2_version}``, ``%{python2_version_nodots}`` or their unversioned alternatives. Substitute these with their counterparts for Python 3, e.g. ``%{python3_sitelib}``.

The files section may also contain the versioned executable, usually ``%{_bindir}/sample-exec-2.7`` in which case it should be substituted by ``%{_bindir}/sample-exec-%{python3_version}``.

Diff of the changes
-------------------

Here is a visualization of the changes to the spec file we have made according to the section :ref:`modifications`.

.. literalinclude:: diffs/application.spec
   :diff: diffs/application.spec.orig
   :caption: Diff between the original example Python 2 spec file and the converted Python 3 spec file.


Ported RPM spec file
--------------------

Finally, here is a fully ported RPM spec file you can peruse at your own pleasure.

.. literalinclude:: diffs/application.spec
   :language: spec
   :caption: Example RPM spec file converted to Python 3
