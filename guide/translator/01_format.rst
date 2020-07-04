Format
======

Before getting into the translation in itself, please pay attention
to the format from the source message.

Links
-----

If the link is to an external page, and there's a version of that
resource in the translated language, then change it to that one. For example,
a link to a book

.. code-block::

    Deans for Impact: "[The Science of Learning]({{ page.root }}/files/papers/science-of-learning-2015.pdf)"


that exists in Spanish would be:

.. code-block::

    Decanos por el Impacto: ["La Ciencia del Aprendizaje"](https://deansforimpact.org/wp-content/uploads/2019/03/LA-CIENCIA-DEL-APRENDIZAJE_FINAL-DFI_1.pdf)


otherwise leave it to the original language.

If the link is different, Transifex will show a warning as you've translated a
link. That's OK.


New lines
---------

Though not essential, try to keep the translation breaking the lines as the
original text. However, it's important that you don't add a new line at the end
of the translation if the source doesn't include one. Note that Transifex won't
let you add the translation if you do so.


Spaces
------

Most of the material being translated in the Carpentries are written in `markdown`_.
Blank spaces at the beginning of the line are important! They mean that are part of
a previous paragraph or section. Please, try to keep them as in the source language.

.. code-block:: text

   ____please work in <https://github.com/carpentries/workshop-template>.


Should be translated into Spanish as:

.. code-block:: text

   ____por favor trabaja en <https://github.com/carpentries-es/workshop-template-es>.


instead of:

.. code-block:: text

   por favor trabaja en <https://github.com/carpentries-es/workshop-template-es>.


YAML front matter block
-----------------------

These refer to the top blocks on Jekyll pages

.. code-block::

   ---
   layout: lesson
   root: .
   permalink: index.html
   ---


By default these should be kept without changes, except the content of tags that
will be rendered like ``title``, ``questions``, ... but not the tag themselves.
For example:

.. code-block::

   ---
   title: "Date and Time"
   teaching: 10
   exercises: 10
   questions:
   - "How can I know what day is today?"
   objectives:
   - "Write a program that calculate the number of hours between two dates."
   keypoints:
   - "Use `timedelta` to store find differences."
   ---

Should be translated in Spanish as:

.. code-block::

   ---
   title: "Fecha y Hora"
   Teaching: 10
   Exercises: 10
   questions:
   - "¿Cómo puedo saber que día es hoy?"
   objectives:
   - "Escribir un programa que calcule el número de horas entre dos fechas."
   keypoints:
   - "Usar `timedelta` para encontrar diferencias."
   ---


Carpentries formatting tags
---------------------------

Many sections are fenced with tags to provide formatting on the rendered lessons,
these tags should not be translated. For example:

.. code-block::

   {: .prereq}


Note that all of these tags uses are followed by blocks of text or code that are
prepended by one or more ``>`` symbols. Keep the same alignment as in the
original text.

.. _markdown: https://daringfireball.net/projects/markdown/syntax


Variables on code samples
-------------------------

Code samples and their references should use meaningful variable names (e.g.,
``age``, ``temperature``) and when working on an international environment we
should use a language that's common between all the collaborators. However, when
learning we should remove the barriers we can to make it easier for learners, so
when possible, we suggest variables are translated to the given language.

.. code-block:: python

   age = 42
   first_name = 'Ahmed'


Translated into Spanish would be:

.. code-block:: python

   edad = 42
   hombre = 'Ahmed'


Functions from libraries should be kept as is, however, if we are creating
a function as part of the lesson they should be translated:

.. code-block:: python

   def calculate_area(radius):
       return np.pi * radius ** 2

   plt.plot(heights, ages)


In Spanish would look like:

.. code-block:: python

   def calcular_area(radio):
       return np.pi * radio ** 2

   plt.plot(alturas, edades)
