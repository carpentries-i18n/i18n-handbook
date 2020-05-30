Download and build
==================


You want to improve this guide? Let's start by building the documentation
locally on your machine!

We will use, `GitHub`_, `git`_, `Python`_, Python's `venv`_ module, and
`Sphinx`_.


.. note::

   Some of the steps below assumes some level of familiarity with the tools. If
   you don't know where to start exactly, do not panic! `Contact me`_ and we can
   go through this together and find out what to add to make this guide even
   better.



Fork and clone the repository
-----------------------------

When contributing to a project hosted on GitHub is a good practice to `fork`_
the project first under your username and `clone`_ your fork locally.

.. code-block:: bash

   git clone https://github.com/<YOUR-USER-NAME>/i18n-handbook.git
   cd i18n-handbook


It's also a good practice to keep an eye what happens ``upstream`` (source
repository) by `configuring a remote`_ on your clone.

.. code-block:: bash

   git remote add upstream https://github.com/carpentries-i18n/i18n-handbook.git



Creating an environment
-----------------------

To build the documentation locally it's recommended that you create an
environment with the dependencies. For example on Linux or OSX you can do as:

.. code-block:: bash

   python -m venv ~/.venv/i18n-sphinx
   source ~/.venv/i18n-sphinx/bin/activate
   pip install -r requirements.txt

refer to the `venv`_ tutorial to see how this is done on Windows.


Build the documentation
-----------------------

Once you've got your environment setup you can go ahead and build the
documentation locally with:

.. code-block:: bash

   sphinx-build -b html . _build


To see what you've built then use:

.. code-block:: bash

   python -m http.server --directory _build/html


That will serve the page on your machine under `<http://localhost:8000>`_ and you
can see from your browser.

.. _GitHub: https://help.github.com/en/github
.. _git: https://swcarpentry.github.io/git-novice/
.. _Python: https://www.python.org/
.. _venv: https://docs.python.org/3/tutorial/venv.html
.. _Sphinx: https://www.sphinx-doc.org/
.. _fork: https://help.github.com/en/github/getting-started-with-github/fork-a-repo
.. _clone: https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository-from-github
.. _configuring a remote: https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/configuring-a-remote-for-a-fork
.. _Contact me: https://github.com/dpshelio
