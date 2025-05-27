Git Introduction
================

* Info about git
* Diagrams to describe structure

References
----------
* `Git Cheatsheet <https://opensource.com/downloads/cheat-sheet-git?intcmp=7013a000002CxqLAAS>`_
* `The Git Book <https://git-scm.com/book/en/v2>`_
* `Git User Manual <https://mirrors.edge.kernel.org/pub/software/scm/git/docs/user-manual.html#the-workflow>`_
* `Steele Git Workflow <https://blog.osteele.com/2008/05/my-git-workflow/>`_
* Mastering Git - Jakub Nrebski
* `Visualising Git <https://www.cs.cmu.edu/~539/notes/notes-visualizing-git-workflows.pdf>`_
* `Git commands with diagrams <https://codewords.recurse.com/issues/two/git-from-the-inside-out>`_
    * Great set of commands described
* `Simple command starter set <https://www.freecodecamp.org/news/learn-the-basics-of-git-in-under-10-minutes-da548267cc91/>`_
* `Git scripts from Git Extras <https://opensource.com/article/20/4/git-extras>`_
    * git-ignore, git-info, git-mr, git-pr, git-release
* /share/sites/internal.sphinx.tw.chm/docs/source/compinfo/git.rst - converted from tiddlywiki to rst
* `Reverse, reset, rebase <https://opensource.com/article/18/6/git-reset-revert-rebase-commands>`_
* `fix merge conflicts <https://www.linuxnix.com/fixing-git-github-merge-conflicts/>`_
* `advanced git tutorial <https://www.howtoforge.com/advanced-git-tutorial/>`_
* `Run your own git server <https://www.linuxfoundation.org/blog/blog/classic-sysadmin-how-to-run-your-own-git-server>`_
* Creating a killer git profile
    * `Part 1 <https://daily.dev/posts/creating-a-killer-github-profile-readme-part-1>`_
    * `Part 2 <https://dev.to/dailydotdev/creating-a-killer-github-profile-readme-part-2-1hie>`_

Notes
----------
* DirName='git';mkdir -p $DirName;cd $DirName;git init --bare random.git #create a new local repo

.gitconfig ::

   [user]
     name = Bob Hacker
	 email = bob@company.com

* Clone from my repository and start working: git clone https://git.company.com/random ; cd random ; git status
* setup my identification
	* git config --global user.name unixgeexen
	* git config --global user.email unixgeexen@gmail.com

Test Git Diagram
-------------------------

Test Git diagram

.. kroki::
   :caption: Git History
   :type: graphviz

    digraph G {
		fontname="Helvetica,Arial,sans-serif"
		node [fontname="Helvetica,Arial,sans-serif"]
		edge [fontname="Helvetica,Arial,sans-serif"]

		subgraph cluster_0 {
			style=filled;
			color=lightgrey;
			node [style=filled,color=white];
			R0 -> R1 -> R2 -> R3;
			label = "Remote Repo";
		}

		subgraph cluster_1 {
			node [style=filled];
			S0 -> S1;
			S1 -> D1 [label = "git branch dev"];
			S1 -> M1 [label = "git branch main"];
			D3 -> S2 [label = "git merge"] ;
			M4 -> S2 [label = "git merge"] ;
			label = "Local Staging";
			color=blue
			subgraph cluster_6 {
				node [style=filled];
				label = "Branch dev";
				D1 -> D2 -> D3;
			}
			subgraph cluster_5 {
				node [style=filled];
				label = "Branch main";
				M1 -> M2 -> M3 -> M4;
			}
		}
	        subgraph cluster_4 {
			node [style=filled];
			color=orange
			label = "Working";
			subgraph cluster_3 {
				node [style=filled];
				T0;T1;T2;T3;
				label = "Tracked";
				color=yellow
			}
			subgraph cluster_2 {
				node [style=filled];
				U0;U2;U3;
				U1 -> U1 [label = "edit"];
				U0 -> U0 [label = "touch"];
				label = "Untracked";
				color=red
			}
			U1 -> T1 [label="git add"];
			T2 -> U2 [label="git rm"];
		}
		start -> R0;
		start -> S0;
		start -> T0;

		R0 -> S0 [label="git pull"];
		S1 -> R1 [label="git push"];
		M3 -> T0 [label="git checkout main"];
		M3 -> T2 [label="git checkout main"];
		M3 -> T3 [label="git checkout main"];
		T0 -> M4 [label="git commit"];
		T1 -> M4 [label="git commit"];
		T3 -> M4 [label="git commit"];


		start [shape=Mdiamond];

    }
