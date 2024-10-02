RTD Tool Chain
==============
Instructions on creating and using the tool chain for generating documentation on ReadTheDocs

References
-----------------
* `RTD Destination Website <https://readthedocs.org/projects/rtdtutorialkroki/>`_
* `Sphinx Quickstart Tutorial <https://sphinx-rtd-tutorial.readthedocs.io/en/latest/install.html>`_
* `Continuous Deployment <https://docs.readthedocs.io/en/stable/integrations.html>`_

Process Outlines
----------------
* create repositories
   * local repository for editing
        * add RTD config `Config <https://docs.readthedocs.io/en/stable/config-file/index.html>`_
   * github repository for editing
   * read the docs repository for reading and publishing
        * Import project
        * add rtd conf
        * build
        * view
* transfer processes
   * update local repository
   * build local site
   * transfer local site to github
   * transfer github site to read the docs
   * update github site
   * pull github back to local site

Commands
--------
* Push to github
    * git remote -v
    * git remote rm origin
    * git log --pretty=oneline --abbrev-commit --all
    * git remote add origin git@github.com:unixgeexen/compinfo.rtd.git
    * git push origin master
