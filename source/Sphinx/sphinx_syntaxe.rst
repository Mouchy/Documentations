.. versionadded:: 1.0
    1 février 2013
.. versionadded:: 1.1
    The linux sphinx installation

sphinx syntaxe
##############

Sphinx memo
***********

Un lien vers `sphinx memo`_

.. _sphinx memo: http://rest-sphinx-memo.readthedocs.org/en/latest/index.html

**Génération de la documentation à partir des fichier RST**

Pour générer la documentation au format HTML à partir de ce premier fichier on ouvre un invité DOS et on se place
	dans le repertoire du projet là ou doit se trouver normalement le fichier et on tape:
	``make html``

Ajouter des documents
*********************

Pour ajouter des documents. Bien sauter une ligne mettre simplement le nom du document sans l'extention qui devra être la même
que celle choisie pendant sphinx-quickstart pour .rst. J'ai donc crée 2 documents intro.rst et sphinx.rst que j'ai placé dans 
mon dossier de projet de documentation. Bien indenter au même niveau que :maxdepth: 2 (pourquoi je ne sais pas trop pour l'instant) 
et avec des espaces pas de tabulation.
	
|   \Contents::\
|
|   \.. toctree::\
|   \:maxdepth: 2\
|   \intro\
|   \sphinx\

RESTRUCTUREDTEXT
****************

Ignorer la transformation RestructuredText
==========================================

.. raw:: html

   .. raw:: html

    Texte HTML à ignorer

Cette syntaxe ne semble pas recommander. De plus je ne sais pas trop comment si le texte est toujours ignoré si je ne génére pas du HTML.

Un autre moyen d'ignorer la transformation RESTRUCTUREDTEXT/sphinx est d'utiliser le marker Source code ::

 ::
 
Un autre moyen d'ignorer la transformation RESTRUCTUREDTEXT/sphinx est d'utiliser le backslash ::

 \** test **\



Sections
========

Pour déclarer des sections souligner le texte avec un des symboles ci-dessous.

| **#** with overline, for parts
| ***** with overline, for chapters
| **=**, for sections
| **-**, for subsections
| **^**, for subsubsections
| **"**, for paragraphs

Paragraphs
==========

Inline markup
=============

one asterisk: *text* for emphasis (italics) ::

 *text* 

two asterisks: **text** for strong emphasis (boldface) ::

 **text**

backquotes: ``text`` for code samples ::

 ``text``

Si un asterisk ou un backote apparait dans le texte pour ne pas le confondre avec une syntaxe Rest il faut le faire précéder d'un backslash \ ::

 \'   \*

\:subscript:`contents`\

:subscript:`contents`

:superscript:`contents`

:title-reference:`contents`

Lists and Quote-like blocks
===========================
lists
-----
List markup (ref9) is natural: just place an asterisk at the start of a paragraph and indent properly. 
The same goes for numbered lists; they can also be autonumbered using a # sign ::

 * This is a bulleted list.
 * It has two items, the second
  item uses two lines.
  
* This is a bulleted list.
* It has two items, the second
  item uses two lines.

Numbered ::

 1. This is a numbered list.
 2. It has two items too.

1. This is a numbered list.
2. It has two items too.
 
Numéroté automatiquement ::

 #. This is a numbered list.
 #. It has two items too.
 
#. This is a numbered list.
#. It has two items too.

Nested lists are possible, but be aware that they must be separated from the parent list 
items by blank lines ::

 * this is
 * a list

  * with a nested list
  * and some subitems
 
 * and here the parent list continues

* this is
* a list

 * with a nested list
 * and some subitems
 
* and here the parent list continues

Definition list
---------------

term (up to a line of text)
  Definition of the term, which must be indented
  and can even consist of multiple paragraphs

next term
  Description.

Paragraphe mise en forme
------------------------

Pour préserver les sauts de ligne on utilise **|** ::

 | These lines are
 | broken exactly like in
 | the source file.

Source code
===========

On utilise le marqueur \::\ avec une ligne vide puis le code que l'on veut afficher ou tout autre texte sans transformation à appliquer à l'excption de la tabulation::

 Mon code ::

 if (test) {
   do
 }

Mon code ::

 if (test) {
   do
 }

Hyperlinks
==========
.. _internal:

external links
--------------



Lien internet externe. La déclaration du label peut etre n'importe ou dans le document ou dans un autre document 
ou bien juste aprés ou avant mais ne pas oublier le saut de ligne entre les deux ::

 This is a paragraph that contains `a link`_.

 .. _a link: http://example.com/

This is a paragraph that contains `a link`_.

.. _a link: http://example.com/

Internal links
--------------

Afin de référencer des liens d'un autre projet, créer un projet principale **temp** puis autant d'autre projet que l'on souhaite.
Lors de la création du projet principal à l'aide de la commande sphinx-quicstart bien répondre oui à la question ::

 intersphinx: link between Sphinx documentation of different projects (y/n) [n]: y
 
Créer ensuite un autre projet **temp2** sans focément répondre oui à la question précédente seul le projet aggrégateur doit être configuré intersphinx.
 
On a donc la strusture de repertoire comme ci-dessous ::
 
 c:\temp\doc_sphinx
 c:\temp2\doc_sphinx
  
Dans le projet sphinx déclaré comme intersphinx ouvrir le fichier conf.py puis remplacer la ligne finale par ::

 # Example configuration for intersphinx: refer to the Python standard library.
 intersphinx_mapping = {'test2':('/test2/build/html', '/test2/build/html/objects.inv'),}  
 
Ensuite utilisé les labels pour définir un point de référence dans le document que l'on souhaite référencer. Ce point de référence doit forcément etre avant une section. 
Pour mon exemple je l'ai positionné entre les sections Hyperlinks et external links et bien penser à mettre un saut de ligne aprés ::

 .. _internal:

Ensuite dans le document ou l'on souhaite avoir le lien utiliser un InLineMarkup ::

 :ref:`internal`
 
Bien faire attention à l'apostrophe utilisée. Elle est différente de l'apostrophe droite standard du clavier utilisé pour intersphinx_mapping.
On est ici sur une apostrophe gauche.

:ref:`internal`

Directives
==========
A directive (ref23) is a generic block of explicit markup. Besides roles, it is one of the extension mechanisms
of reST, and Sphinx makes heavy use of it.
An explicit markup block begins with a line starting with .. followed by whitespace and is terminated
by the next paragraph at the same level of indentation.

Admonitions
-----------

Docutils supports the following directives ::

 .. ATTENTION::
    This is an attention admonition

.. ATTENTION::
   This is an attention admonition

::

 .. DANGER::
   Beware killer rabbits!

.. DANGER::
   Beware killer rabbits!

::

 .. CAUTION::
   This is a caution Admonition

.. CAUTION::
   This is a caution Admonition

::

 .. ERROR::
    This is an error admonition

.. ERROR::
    This is an error admonition

::

 .. HINT::
    This is an hint admonition

.. HINT::
    This is an hint admonition

::

 .. IMPORTANT::
    This is an important admonition

.. IMPORTANT::
    This is an important admonition

Images
------

Image
^^^^^

::

 .. image:: Images/sphinxheader.png
   :height: 100px
   :width: 200 px
   :scale: 50 %
   :alt: alternate text
   :align: right

.. image:: Images/sphinxheader.png
   :height: 100px
   :width: 200 px
   :scale: 50 %
   :alt: alternate text
   :align: left

Texte suivant l'image

Figure
^^^^^^

An image with caption and optional legend ::

 .. figure:: Images/sphinxheader.png
   :scale: 50 %
   :alt: map to buried treasure

.. figure:: Images/sphinxheader.png
   :scale: 50 %
   :alt: map to buried treasure

Texte suivant la figure


Additional body elements
------------------------
Contents
^^^^^^^^

::

 .. contents:: Table of Contents
    :depth: 2

.. contents:: Table of Contents
   :depth: 2

The following options are recognized:
depth : integerThe number of section levels that are collected in the table of contents. The default is unlimited depth.

local : flag (empty)Generate a local table of contents. Entries will only include subsections of the section in which the directive is given. 
If no explicit title is given, the table of contents will not be titled.

backlinks : "entry" or "top" or "none"Generate links from section headers back to the table of contents entries, 
the table of contents itself, or generate no backlinks.

class : textSet a "classes" attribute value on the topic element. See the class directive below.

Container
^^^^^^^^^
Option à creuser ::

 .. container:: custom

   This paragraph might be rendered in a custom way.

.. container:: custom

   This paragraph might be rendered in a custom way.

Rubric
^^^^^^

Pas compris comment d'en servir pour l'instant

rubric n. 1. a title, heading, or the like, in a manuscript, book, statute, etc., written or printed in red or otherwise distinguished from the rest of the text

Topic
^^^^^
::

 .. topic:: Topic Title

    Subsequent indented lines comprise
    the body of the topic, and are
    interpreted as body elements.


.. topic:: Topic Title

    Subsequent indented lines comprise
    the body of the topic, and are
    interpreted as body elements.

Sidebar
^^^^^^^
.. sidebar:: Sidebar Title
   :subtitle: Optional Sidebar Subtitle

   Subsequent indented lines comprise
   the body of the sidebar, and are
   interpreted as body elements.
   
::

 .. sidebar:: Sidebar Title
   :subtitle: Optional Sidebar Subtitle

   Subsequent indented lines comprise
   the body of the sidebar, and are
   interpreted as body elements.

Substitutions
^^^^^^^^^^^^^
Substitutions are a useful way to define a value that’s needed in many places (eg. a command, the location of a file, etc.) 
in one place and then reuse it many times.

You define the value once like this ::

 .. |production.ini| replace:: /etc/ckan/default/production.ini


and then reuse it like this ::

 Now open your |production.ini| file.
 |production.ini| will be replaced with the full value /etc/ckan/default/production.ini.

Now open your |production.ini| file.
|production.ini| will be replaced with the full value /etc/ckan/default/production.ini.

parsed-literal
^^^^^^^^^^^^^^

Le texte qui suit cette balise est parsé pour lui substituer les éléments inline markup. 
Par exemple tous les elements qui suivent sont des liens ::

 .. parsed-literal::

   ( (title_, subtitle_?)?,
     decoration_?,
     (docinfo_, transition_?)?,
     `%structure.model;`_ )

Exemple ::

 .. |production.ini| replace:: /etc/ckan/default/production.ini
 .. |development.ini| replace:: /etc/ckan/default/development.ini

 .. parsed-literal::

   cp |development.ini| |production.ini|

.. |production.ini| replace:: /etc/ckan/default/production.ini
.. |development.ini| replace:: /etc/ckan/default/development.ini

.. parsed-literal::

   cp |development.ini| |production.ini|

Epigraph
^^^^^^^^

::

 .. epigraph::

   No matter where you go, there you are.

   -- Buckaroo Banzai

.. epigraph::

   No matter where you go, there you are.

   -- Buckaroo Banzai

Highlights
^^^^^^^^^^


Pull-Quote
^^^^^^^^^^

::

 .. note:: This is a note a
   This is the second line

.. note:: This is a note a
   This is the second line

Sphinx
******

Code highlighting
=================

Avec reST On utilise ::

 .. highlight:: ‹language›
    :linenothreshold: ‹number›

La prochaine fois que l'on utilise  **::** on obtiendra la sortie avec un code de type **C** et 
l'option linenothreshold va commencer une numértation dés que le code dépasse 5 lignes ::

 .. highlight:: c
    :linenothreshold: 5

.. highlight:: c
    :linenothreshold: 5

Exemple::
 
  if (testb) {
    a = b;
 }

When using Sphinx you can specify the highlighting in a single literal block ::

 .. code-block:: ‹language›
    :linenos:

   ‹body›

Sphinx Domains
==============

Il existe plusieurs domaine gere pars sphinx. Le pyton, le javascript, le C, le C++ et le reST lui meme.

.. _sphinx domain: http://sphinx-doc.org/domains.html

The C Domain
------------

The C domain (name c) is suited for documentation of C API ::

 .. c:function:: type name(signature)

Exemple ::

 .. c:function:: APSCHAR *st_cat(APSCHAR *p, const APSCHAR *b)

Donnera

.. c:function:: APSCHAR *st_cat(APSCHAR *p, const APSCHAR *b)

Describes a C struct member ::

 .. c:member:: type name

Describes a C struct member. Example signature::

 .. c:member:: PyObject* PyTypeObject.tp_bases

.. c:member:: PyObject* PyTypeObject.tp_bases

Describes a “simple” C macro. Simple macros are macros which are used for code expansion, but which do not take arguments 
so cannot be described as functions. This is a simple C-language #define. 
Examples of its use in the Python documentation include PyObject_HEAD and Py_BEGIN_ALLOW_THREADS ::

 .. c:macro:: name
 .. c:macro:: RERR002_RETOUR_INI_MARGE_INIT

.. c:macro:: RERR002_RETOUR_INI_MARGE_INIT

Describes a C type (whether defined by a typedef or struct). The signature should just be the type name ::

 .. c:type:: name
 .. c:type:: rerctrretro_

.. c:type:: rerctrretro_

Describes a global C variable. The signature should include the type, such as::

 .. c:var:: type name
 .. c:var:: double* mtbased
 
.. c:var:: double* mtbased



Cross-referencing C constructs

The following roles create cross-references to C-language constructs if they are defined in the documentation::
 
 :c:data:
 
Reference a C-language variable ::

 :c:func:
 
Reference a C-language function. Should include trailing parentheses ::

 :c:macro:
 
Reference a “simple” C macro, as defined above ::

 :c:type:
 
Reference a C-language type.

Info field lists
----------------

Inside Python object description directives the following fields are recognized: 
param, arg, key, type, raises, raise, except, exception, var, ivar, cvar, returns, return, rtype

.. sidebar::
   ..  function:: divide( i, j)

    divide two numbers

    :param i: numerator
    :type i: int
    :param j: denominator
    :type j: int
    :return: quotient
    :rtype: integer
    :raises: :exc:`ZeroDivisionError`

..  function:: divide( i, j)

    divide two numbers

    :param i: numerator
    :type i: int
    :param j: denominator
    :type j: int
    :return: quotient
    :rtype: integer
    :raises: :exc:`ZeroDivisionError`

f


Source code docstring
---------------------

.. sidebar::
   def example(self, arg1, arg2):
    Docstring example.

    :Parameters:
      - `arg1` (float) - the first value
      - `arg2` (float) - the second value

    :Returns:
        arg1/arg2

    :Returns Type:
        float

    :Examples:

    >>> import template
    >>> a = MainClass()
    >>> a.example(6,2)
    1.5

    .. note:: can be useful to emphasize
       important features.
    .. seealso:: :py:exc:`ZeroDivisionError`
    .. warning:: arg2 must be non-zero.
    .. todo:: check that arg2 is non zero.

def example(self, arg1, arg2):
    Docstring example.

    :Parameters:
      - `arg1` (float) - the first value
      - `arg2` (float) - the second value

    :Returns:
        arg1/arg2

    :Returns Type:
        float

    :Examples:

    >>> import template
    >>> a = MainClass()
    >>> a.example(6,2)
    1.5

    .. note:: can be useful to emphasize
       important features.
    .. seealso:: :py:exc:`ZeroDivisionError`
    .. warning:: arg2 must be non-zero.
    .. todo:: check that arg2 is non zero.



The reStructuredText domain
---------------------------
The reStructuredText domain (name rst) provides the following directives:
.. rst:directive:: name

Describes a reST directive. The name can be a single directive name or actual directive syntax (.. prefix and :: suffix) 
with arguments that will be rendered differently. For example ::

 .. rst:directive:: ATTENTION
    This is an attention admonition

will be rendered as:


.. rst:directive:: ATTENTION

    This is an attention admonition

Ou bien ::

 .. rst:directive:: .. image:: /path/picture.png

   Affiche une image

Donnera

.. rst:directive:: .. image:: /path/picture.png

   Affiche une image


Describes a reST role .. rst:role:: name . For example::

 .. rst:role:: foo

   Foo description.


will be rendered as ::

 :foo:
 Foo description.

These roles are provided to refer to the described objects ::

 :rst:dir::rst:role:

**Quick reStructuredText guide**  
  
http://docutils.sourceforge.net/docs/user/rst/quickref.html

Changer de théme trouver le fichier conf.py  trouver la ligne commençant par html_theme et remplacer  aprés le égal par la ligne voulut

html_theme = 'default'


exemple de thèmes
http://sphinx-doc.org/theming.html


**installation eclipse**

Like other plug-ins :

Go to Help> Install new software

Add the project update site : http://resteditor.sourceforge.net/eclipse

Select the ReST Editor plug-in.

The Eclipse Color Theme plug-in may not be accessible with default update sites, so you may un-check it if you don’t use this one. 





 
  
 

