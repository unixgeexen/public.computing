Git Recipes
==============================

References
----------
* `request-pull man <https://git-scm.com/docs/git-request-pull>`_

Recipes
------------------------------
* Sample settings
   * DevDir=/share/sites/internal.sphinx.org
   * BranchName=feature/FeatureName
   * GitRepo=https://github.com/v1s1t0r1sh3r3/airgeddon.git
   * NewFile=newfile.data
* Delete Branch
   * git push --delete origin ${BranchName}
* Create Branch
   * mkdir -m 755 ${DevDir}/${BranchName}
   * cd ${DevDir}/${BranchName}
   * git clone --branch ${BranchName} ${GitRepo}
* Update branch
   * vi ${NewFile}
   * git add ${NewFile}
   * git commit -am 'Update Info'
   * git push origin ${BranchName}
   * git request-pull [-p] <start> <URL> [<end>]
      * git push https://git.ko.xz/project master
      * git request-pull v1.0 https://git.ko.xz/project master
