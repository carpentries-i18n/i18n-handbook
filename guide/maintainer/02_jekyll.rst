Working with Jekyll lessons
===========================

.. note::

   Some of the steps below needs to be run by someone with write access to the
   `carpentries-i18n`_ organisation.


Prepare the lesson
------------------

To prepare a jekyll-based lesson for translation we need few steps:

#. Fork the repository under the `carpentries-i18n`_ organisation.
#. Set-it up to use the Jekyll-theme (i.e., remove everything that's not lesson content).
#. Create it as a submodule within the :org-repo:`i18n` repository.
#. Generate the `po` files using :org-repo:`po4gitbook` software.
#. Since `po4gitbook` generates the whole lesson as a single file, break it into episodes with ``splitpot.py``.
#. Create the project on Transifex and push the source files.

The first three steps can be done automatically using ``lesson2theme.py`` from
:org-repo:`i18n`. You'll need to `create an access token on GitHub`_ first.

.. code-block:: bash

   $ cd i18n
   i18n (git)-[master]$ export gh_access_token=xxxxxxxx
   i18n (git)-[master]$ python helpers/lesson2theme.py swcarpentry/python-novice-gapminder
   i18n (git)-[python-novice-gapminder]$


That command:

#. forks the repository,
#. fixes any recognised formatting issues that will affect the conversion to PO
   (e.g., :commit-png:`52cacd8`),
#. removes everything that's provided by the theme (e.g., :commit-png:`682a376`),
   and
#. updates ``_config.yml`` so it accepts multiple languages (e.g.,
   :commit-png:`f4e6e3b`).
#. Then adds the forked repo as a submodule to :org-repo:`i18n` repository in a
   branch with the lessons name.

The output of the above command gives you the link of the forked repository so
the result can be checked manually with a list of the following steps that are
to be done manually.

The first of these steps is to run ``po4gitbook`` to update/generate the ``po`` files.

.. warning::

   If the command gets stuck for more than a couple of seconds that's due to
   some formatting issues as the once fixed on the second step above.

.. note::

   The ``update.sh`` goes through all the md files, finds whether there's been
   an update on the sources and propagates to new files, if they've changed
   tries to merge them and marks the blocks as fuzzy if the source has changed.

Now you should have a new file under the ``po`` directory:

.. code-block:: bash

   i18n (git)-[python-novice-gapminder]$ po4gitbook/update.sh
   ...
   i18n (git)-[python-novice-gapminder]$ ls po
   ...
   python-novice-gapminder.pot
   ...

.. _jekyll-chunks:

That ``pot`` file is the template that we will use to break it up into chunks
first and then send these to transifex.

.. code-block:: bash

   i18n (git)-[python-novice-gapminder]$ python helpers/splitpot.py po/python-novice-gapminder.pot
   ...
   i18n (git)-[python-novice-gapminder]$ ls transifex/python-novice-gapminder/pot
   00__CODE_OF_CONDUCT.md.pot  06__03-types-conversion.md.pot  12__09-plotting.md.pot           18__15-coffee.md.pot             24__about.md.pot      30__aio.md.pot
   01__CONTRIBUTING.md.pot     07__04-built-in.md.pot          13__10-lunch.md.pot              19__16-writing-functions.md.pot  25__design.md.pot     31__index.md.pot
   ...


Then we need to create the target language directory we want (e.g., ``es`` for
Spanish), and let Transifex's command line tool (``tx``) to prepare the files

.. code-block:: bash

   i18n (git)-[python-novice-gapminder]$ mkdir -p transifex/python-novice-gapminder/es
   i18n (git)-[python-novice-gapminder]$ cd transifex/python-novice-gapminder
   python-novice-gapminder (git)-[python-novice-gapminder]$ cd transifex/python-novice-gapminder
   python-novice-gapminder (git)-[python-novice-gapminder]$ tx config mapping-bulk -p python-novice-gapminder --source-language en --type PO -f '.pot' \
                   --source-file-dir pot --expression "<lang>/{filename}.po" --execute


The last command generates a ``config`` file under a hidden ``.tx`` directory.
We need then to `add the project in Transifex`_ where we need to input a name
(same as the lesson), select that's a public project, add the url of the
project, select that's a file-based project, assign it to the
``carpentries-translation`` team and select the target languages (by default it
adds all that we've used before).

.. image:: images/Transifex_add_project.png
   :width: 600
   :alt: Sample of how to fill up Transifex form.


Once the project is created in transifex we can push the project using ``tx``:

.. code-block:: bash

   python-novice-gapminder (git)-[python-novice-gapminder]$  tx push -s --parallel

Once the upload has been completed, you should see the resources available in
the project page in Transifex (e.g., `python-novice-gapminder
<https://www.transifex.com/carpentries-i18n/python-novice-gapminder/dashboard/>`_)

Finally, add the ``<lesson>.pot`` file to the repository and push it to GH for
review and merge with ``master``.


.. warning::

   Some times (e.g., :issue-p4g:`6`) you may encounter that something fails in
   the process. If you encounter a similar problem, please add it as an issue to
   the right repository. If you don't know which one is the correct one, then
   add it to :org-repo:`i18n`.


Bring the translations to the rendered page
-------------------------------------------

Once a lesson has been translated on Transifex (or you want to see its
progress), we can pull the files as:

.. code-block:: bash

   i18n (git)-[python-novice-gapminder]$ language="es"
   i18n (git)-[python-novice-gapminder]$ lesson="python-novice-gapminder"
   python-novice-gapminder (git)-[python-novice-gapminder]$ pushd transifex/${lesson}
   python-novice-gapminder (git)-[python-novice-gapminder]$ ls -F
   es/  pot/
   python-novice-gapminder (git)-[python-novice-gapminder]$ tx pull -l ${language} --parallel
   ...
   [#########################] 100% (34/34)
   tx INFO: Done.


The ``tx`` command line offers multiple options as to define a minimum
percentage of translations (``--minimum-perc``), chose only files that has been
reviewed (``--mode``). Check them with ``tx pull --help``.

Once we have all the translated files downloaded we need to merge them to a
single file for the whole lesson (i.e., the opposite of what we did
:ref:`previously <jekyll-chunks>`).

.. code-block:: bash

   python-novice-gapminder (git)-[python-novice-gapminder]$ popd
   i18d (git)-[python-novice-gapminder]$ python helpers/splitpot.py po/${lesson}.pot --join transifex/${lesson} --lang ${language}


Now we've got all the chunks into a single file named:
``{lesson}.{language}.po`` under the ``po`` directory.

Next we need to generate the markdown files. To do so we use the `po4gitbook`
tool.

.. code-block:: bash

   i18d (git)-[python-novice-gapminder]$ ../po4gitbook/compile.sh


.. note::

   The ``compile.sh`` will throw errors if the format of the ``po`` file is not
   right (e.g., if there's not a period after each year in the translators list).


This command will generate all the markdown files of the lesson in a new
directory under the ``locale/{language}/lesson`` directory.

The next step is integrate these new files with the original lesson so the source
lesson and its translations get rendered under the same page. This requires the
following steps:

1. move the new files to its own repository: ``{lesson}-{language}`` and upload
   it into github
1. Add that new repository as a sub-module of the source lesson: ``{lesson}/_locale/{language}``
1. The website should now show both languages in the same page

This helper tool automates these steps

.. code-block:: bash

   i18d (git)-[python-novice-gapminder]$ python helpers/trans2lesson.py locale/es/python-novice-gapminder python-novice-gapminder
   Repository https://github.com/carpentries-i18n/python-novice-gapminder.git updated with es.


.. todo::

   Find the way to Credit translators



.. _carpentries-i18n: https://github.com/carpentries-i18n
.. _create an access token on GitHub: https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line
.. _add the project in Transifex: https://www.transifex.com/carpentries-i18n/add/

