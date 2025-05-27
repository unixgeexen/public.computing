Sphinx Documentation Notes
==============================

References
----------
* `Local Site <http://0.0.0.0:8080/public/computing/docs/build/html/index.html>`_
* `GitHub Repo <https://github.com/unixgeexen/public.computing/tree/main>`_
* `ReadTheDocs Management - unixgeexen <https://readthedocs.org/dashboard/>`_
* `ReadTheDocs Output <https://publiccomputing.readthedocs.io/en/latest>`_

TODO
----

* Document Management Simplification
    * making use of vscode for documentation processes
    * handling different source directories - sphinx, docs, git - aliases?
* Create sample starter configurations to fork
    * python project documentation
    * standard written documentation
    * fix lumarche in starter
    * set up working kroki config for full tool chain
* Document processes
    * updates in public vs on local system and merging
    * how frequently to push out documentation changes
    * adding new documentation directory
    * create a new set of documentation - userid, public/private, new local dir/github repo/readthedocs user/readthedocs project
    * fork starter configuration and process through to RTD output
    * managing upstream and origin configurations - merging from upstream
* Documentation Features
    * Using tags
        * Sphinx Tags <https://sphinx-tags.readthedocs.io/en/latest/index.html>`_
* Add existing data to new structure
    * Convert tiddlywiki, evaluate content and move to appropriate new structure

Procedures 
------------------------------

Add New Documentation Directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* mkdir NewDir
* add NewDir to higher level index.rst - NewDir/index.rst
* cd NewDir
* cp index.rst
* add notes.rst
* add git configuration
