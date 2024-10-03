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
        * `Sample kroki RTD for antora <https://github.com/man-chi/foundation/blob/main/.readthedocs.yaml>`_
        * `lims sample <https://github.com/genespace-ru/lims-docs/blob/1d139c074e948462cd1a209dfc217c222879febc/requirements.txt>`_
        * `CERN RTD.yaml <https://github.com/CERN/Quantumacy/blob/main/.readthedocs.yaml>`_
* Document processes
    * how frequently to push out documentation changes
    * adding new documentation directory
    * create a new set of documentation - userid, public/private, new local dir/github repo/readthedocs user/readthedocs project
    * fork starter configuration and process through to RTD output
    * managing upstream and origin configurations - merging from upstream

Procedures 
------------------------------

Add New Documentation Directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* mkdir NewDir
* add NewDir to higher level index.rst - NewDir/index.rst
* cd NewDir
* cp index.rst
* add notes.rst
