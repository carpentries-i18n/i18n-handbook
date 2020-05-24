Understanding the structure
===========================

At the moment there are translation support for two types of sources:

- carpentries lessons using `jekyll`_ (:project:`1`)
- carpentries documentation using `sphinx`_ (:project:`4`)

The other type of material is the lessons based on |rmarkdown|_. There is the
:project:`2` with issues to tackle the implementation of that type of lessons.

The goal is that everything is translated using `gettext`_ system. This system
converts the source product into a ``PO`` file format that many translation
software understand and it is therefore easier to update when the source changes
and find what bits are new. All the issues related with the tooling and
infrastructure is at the :project:`3`.

The Carpentries-i18n organisation
---------------------------------

`This organisation`_ includes, for now forks of all the lessons and other
repositories with code needed to convert, automate and visualise the lessons and
their translations.

.. note::
   At the moment the material and all the repositories in this organisation is
   not officially under `The Carpentries`_ umbrella. However, the people behind
   this effort are part of the Carpentries' community.

The mains repositories are:

- :org-repo:`i18n-handbook` - The source of this documentation.
- :org-repo:`i18n` - The machinery to support the ``jekyll`` lessons.
- :org-repo:`carp-theme` - A `Jekyll theme`_ to render the lessons with multilingual support
- :org-repo:`handbook-translations` - Translations repository for the
  `Carpentries handbook`_ that uses sphinx.
.. _jekyll: https://jekyllrb.com/
.. _sphinx: https://www.sphinx-doc.org/
.. |rmarkdown| replace:: ``rmarkdown``
.. _rmarkdown: https://rmarkdown.rstudio.com/
.. _gettext: https://en.wikipedia.org/wiki/Gettext
.. _This organisation: https://github.com/carpentries-i18n
.. _The Carpentries: https://carpentries.org/
.. _Jekyll theme: https://jekyllrb.com/docs/themes/
.. _Carpentries handbook: https://docs.carpentries.org/
