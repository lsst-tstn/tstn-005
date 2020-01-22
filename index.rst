..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the master branch.

.. note::

   **This technote is not yet published.**

   This document guides a member of the TSSW Team to the correct location for their documentation. 

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).
.. _create-your-own-technote:

Create Your Own Technote
========================

Creating your own technote just like this one is simple. Here we will explore how to create one.

1. Become a member of the `tstn <https://github.com/lsst-tstn>`_ organization. Ask any current members to add you. 

2. Create your repository. Go to Slack and message **create project** to **sqrbot-jr**. You will then answer a series of questions.

    title
      The name of the tech note
    abstract
      A paragraph or less summary about the document
    series
      The technote series(team based), choose tstn(Telescope and Site Tech Note)
    Initial Copyright Holder
      The copyright holder for the document. Choose AURA( Association of Universities for Reseach in Astronomy)

.. note::

   Before continuing with the steps check out the github repo you just created. It has a readme with lots of useful information such as a link to where your website is hosted. 

3. Perfect, your boilerplate repo is created. Now pull down the repo on your local machine. If you are struggling to find the URL to pull you can find the series number at the end of sqrbot-jr's successful creation of the repo.

.. prompt:: bash

   git clone https://github.com/lsst-tstn/tstn-{series_number}.git

4. Install miniconda on your local machine so we can create a python environment that is able to build your website. Otherwise you will need to push and commit every single change to see changes to the site and that's silly. Here's the site link https://docs.conda.io/en/latest/miniconda.html.

5. Create and activate a miniconda environment for your documentation work. Python 3.6 should work fine.

.. prompt:: bash

   conda create --name documentation_env python=3.6
   source activate documentation_env

6. Now inside your miniconda environment install the python packages you need to build the static website. If you open the requirements.txt you will find that the only item there is `Documenteer <https://documenteer.lsst.io>`_ which is a DM provided package for generating websites. It comes with a nice theme and other boilerplating for the website.

.. prompt:: bash

   pip install -r requirements.txt

7. pip install any extensions you want to use. Before creating this documentation I knew I wanted to create some UML diagrams and plant UML was something I wanted to try out. You may also find out some extensions such as prompt are already installed via Documanteer.

.. prompt:: bash

   pip install sphinxcontrib-plantuml
   pip install sphinx-prompt

8. The packages are installed in our python environment. We now need to just tell our project that we want to use it. Inside of the conf.py add the following line at the end. This is sphinx style syntax where if you want to include libraries you need to add it to your extensions variable. Also Make sure you add any sphinx extensions to the requirements.txt file for future developers and Travis CI as builds can break.

.. code::

   extensions = ['sphinxcontrib.plantuml', 'sphinx-prompt']

9. Cool, you have documanteer and maybe some other python packages neatly tucked inside a python environment. You should be able to build your static website now.

.. prompt:: bash

   make html

10. If all went well you can now open the generated _build/html/index.html file. When you are happy with your local site push your changes. You can view the link to your site on the github repo.

.. _configure-your-repo-for-docstrings:

Configure your Repo for Docstrings
==================================

There are two ways to configure your repo for docstrings.
If you are already using the python repo template from lsst/template repo, then the doc folder is already setup for you.
You can build documentation according to the rst standards.
However, if you have manually setup a repo, then you will need to prepare it up manually.

Inside of your code repo, create a folder called `doc`.
Inside of the new folder create a series of files `index.rst` `conf.py` `requirements.txt` `.gitignore`.
::

  ├── .gitignore
  ├── conf.py
  ├── index.rst
  ├── requirements.txt

Your directory structure should look similar to this one.

Open `requirements.txt` and write in the following.

.. code:: text

  documenteer[pipeline]

Open .gitignore and write in the following

.. code::

  # Sphinx products
  _build
  py-api

Open `conf.py` and write in the following.

.. code:: python

  from documenteer.sphinxconfig.stackconf import build_package_configs
  import lsst.ts.{PACKAGE_NAME}


  _g = globals()
  _g.update(build_package_configs(
      project_name='{NAME_OF_PROJECT}',
      version=lsst.ts.{PACKAGE_NAME}.__version__))

Once this is finished, you can start filling in `index.rst` according to the rst guide.
Adding the following will allow you to see your generated docstrings.

.. code::

  .. automodapi:: lsst.ts.{PACKAGE_NAME}
    :no-main-docstr:
    :no-inheritance-diagram:

Some advice for making this easier is to use in `__init__.py` 

.. code:: python

  from .csc import *
  from .controller import *

This will have the benefit of adding all those classes to the main package namespace and therefore generate 
the docstrings for all of those classes.
You can build your docstrings by running the following in the root package directory

.. prompt:: bash

  package-docs build

You can then preview your work by opening index.html in a web browser like so.

.. prompt:: bash

  open index.html
.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
