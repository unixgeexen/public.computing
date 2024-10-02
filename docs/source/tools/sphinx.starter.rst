Sphinx RTD Starter
==================

Introduction
------------
* How to use sphinx for documentation using RTD (Read The Docs) Theme
* Implementation of pipeline from local to shared GitHub/RTD sites
* Multiple document types with multiple users

References
----------
* `Sphinx-RTD Tutorial <https://sphinx-rtd-tutorial.readthedocs.io/en/latest/read-the-docs.html>`_
* `Lumache Starter (not RTD) <https://www.sphinx-doc.org/en/master/tutorial/getting-started.html>`_
* `Simple tutos config <https://tutos.readthedocs.io/>`_
* Continuous Docmentation - GitHub - RTD
    * `Continuous Docmentation - GitHub - RTD #1 <https://tech.michaelaltfield.net/2020/07/18/sphinx-rtd-github-pages-1/>`_
        * Great ideas and scripts for full pipeline local > github > rtd
    * `Continuous Docmentation - GitHub - RTD #2 <https://tech.michaelaltfield.net/2020/07/23/sphinx-rtd-github-pages-2/>`_
        * Ideas for versioning and internationalisation

Required Repos
--------------
* Separation factors
    * Public vs Private - no GitHub RTD for private
        * /share/sites/public
        * /share/sites/private
    * Computing vs non-Computing 
        * /share/sites/*/computing
        * /share/sites/*/work
        * /share/sites/*/home

Current Repos
--------------------------
Functioning
~~~~~~~~~~~
* https://unixgeexen-demo.readthedocs.io/en/latest/
    * https://readthedocs.org/dashboard/
    * https://github.com/readthedocs/template.git
* https://unixgeexen.github.io/rtd-github-pages/ # Altfield as github pages
* https://sphinx-rtd-tutorial.readthedocs.io/en/latest/ # Howto

Non-Functioning
~~~~~~~~~~~~~~~
* compinfo.rtd https://github.com/unixgeexen/compinfo.rtd.git

Sphinx RTD simpleble Tutorial
-----------------------------
* cd /share/sites/sphinx
* python -m venv simpleble.tutorial
* source simpleble.tutorial/bin/activate
* python -m pip install sphinx sphinx-rtd-theme bluepy
* python -m pip freeze>simpleble.tutorial.requirements.txt
* git clone https://github.com/sglvladi/simpleble
    * repository root - simpleble
    * Sphinx root - simpleble/docs
    * Sphinx source root - simpleble/docs/source
    * Sphinx build root - simpleble/docs/build
* cd simpleble
* cd docs
* make html
* setup .gitignore as below
* git config --global --add safe.directory /share/sites/sphinx/simpleble
* git add .
* git commit -am init
* git remote set-url origin git@github.com:unixgeexen/sphinx-rtd-simpleble.git
* git push
* readthedocs > import project > refresh > select github repo
* Also see https://docs.readthedocs.io/en/latest/integrations.html#continuous-documentation-deployment

Create Repo Locally and Push to GitHub
--------------------------------------
* git init
* vi README.md
* git add .
* git commit -am "initial commit"
* GitHub > New Repository > Name > description > Create Repository
* Copy project URL blah.git
* git remote add origin blah.git
* git remote -v
* git push origin master
* git remote set-url origin git@github.com:user/repository.git

Fork Repo - access both local origin and upstream
-------------------------------------------------
* References
    * `GitHub Fork Doc <https://docs.github.com/en/get-started/quickstart/fork-a-repo>`_
* Process Summary
    * Fork upstream from GitHub web interface - fork button
    * Get URL from your GitHub fork
    * Clone your fork to local git clone https://github.com/YOUR-USERNAME/Spoon-Knife
    * Add upstream remote to local git envt git remote add upstream https://github.com/ORIGINAL-OWNER/Spoon-Knife.git
    * git remote -v # should show both upstream and origin remotes
    * Sync fork with upstream https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork
        * git fetch upstream
        * git checkout main
        * git merge upstream/main
    * Update upstream using pull request

Simple test setup and run
--------------------------
* SphinxDir=sphinx
* SphinxBaseDir=/share/sites/${SphinxDir}
* cd $SphinxBaseDir
* SphinxEnvt=base
* python -m venv sphinx.${SphinxEnvt}
* source sphinx.${SphinxEnvt}/bin/activate
* InstallPackages="sphinx"
* python -m pip install ${InstallPackages}
* sphinx-build --version # check installation
* ProjectName=Quickstart
* ProjectAuthor=unixgeexen
* mkdir "${ProjectName}"
* cd "${ProjectName}"
* sphinx-quickstart --sep --project ${ProjectName} --author ${ProjectAuthor} --release 0.1 --language en . # create documentation layout
* sphinx-build -M html source/ build/
* ps -ef|grep http.server|grep -v grep
* starthttp
* echo "Browse to http://0.0.0.0:8080/${SphinxDir}/${ProjectName}/build/html/index.html "
* `Browse site <http://0.0.0.0:8080/sphinx/docs/build/html/index.html>`_
* firefox -new-window http://0.0.0.0:8080/${SphinxDir}/${ProjectName}/build/html/index.html
* make html
* deactivate
* cd $SphinxBaseDir
* echo "# rm -rf ${SphinxBaseDir}/${ProjectName} # Clean up project"
* echo "# rm -rf ${SphinxBaseDir}/sphinx.${SphinxEnvt} # Clean up virtual env"

sphinx rtd base setup
--------------------------
* InstallPackages="sphinx sphinx_rtd_theme"
* python -m pip install ${InstallPackages}
* source/conf.py - html_theme = 'sphinx_rtd_theme'

sphinx rtd kroki setup
--------------------------
* InstallPackages="sphinx sphinx_rtd_theme setuptools sphinxcontrib-kroki"
* python -m pip install ${InstallPackages}
* source/conf.py - html_theme = 'sphinx_rtd_theme'

requirements.txt
----------------
* python -m pip install freeze > /share/sites/sphinx/sphinx.kroki.requirements.txt # save requirements for an environment
* python -m pip install -r /path/to/requirements.txt # define in this environment

git configuration
--------------------------

* sudo apt-get install -y git
* mkdir rtd-github-pages
* cd rtd-github-pages/
* mkdir docs
* git init

.. code-block:: console

   cat > docs/.gitignore <<EOF
   *.swp
   /_build
   /doctrees
   EOF
 
.. code-block:: console

   cat > .gitignore <<EOF
   __pycache__
   *.pyc
   EOF

github configuration
--------------------------
maltfield config
~~~~~~~~~~~~~~~~
* fork `GitHub Repo <https://github.com/maltfield/rtd-github-pages/tree/master>`_
    * Dropdown fork > enter your github repository > fill in details > create fork
    * Your repo > settings > Pages > Source (GitHub Actions)
    * mkdir ; cd
    * git clone git@github.com:unixgeexen/rtd-github-pages.git

rtd configuration
---------------------
* https://readthedocs.org/dashboard/
* import a project
* .readthedocs.yaml - requirements: doc/requirements.txt # in top level of Git repo

