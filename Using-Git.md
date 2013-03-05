GIT Overview
============

Git is a free & open source, distributed version control system designed
to handle everything from small to very large projects with speed and
efficiency. Every Git clone is a full-fledged repository with complete
history and full revision tracking capabilities, not dependent on
network access or a central server. Branching and merging are fast and
easy to do.

In the rest of the document I will talk about URL’s to clone the code.
There a few URL’s you can use to clone a project, using https, ssh and
git. You can use either https or git to clone a repository and write to
it. The git protocol is read-only.

PEcAnProject GitHub location:
[https://github.com/organizations/PecanProject](https://github.com/organizations/PecanProject)

PEcAn code
[https://github.com/PecanProject/pecan.git](https://github.com/PecanProject/pecan.git)
[git@github.com](mailto:git@github.com):PecanProject/pecan.git

BETY code
[https://github.com/PecanProject/bety.git](https://github.com/PecanProject/bety.git)
[git@github.com](mailto:git@github.com):PecanProject/bety.git

Useful References
-----------------

# [GitHub Flow](http://scottchacon.com/2011/08/31/github-flow.html) by
Scott Chacon
 * (Scott Chacon is “Git evangelist and Ruby developer working on
GitHub.com”)
# “Stackoverflow highest voted questions tagged”git“”

Quick Start:
------------

Introduce yourself to GIT

    git config --global user.name "FULLNAME"
    git config --global user.email you@yourdomain.example.com

Get the remote repository locally:

    git clone URL

To update local repository to latest:

    git pull

To add new files to to local repository:

    git add <file>

To commit changes

    git commit <file|folder>

To update remote repository:

    git push

Show graph of commits:

    git log --graph --oneline --all

GitHub and fork
---------------

You can fork a project on github. This allows you to create your own
copy of the code. When you do the fork a copy of the code is created and
placed in your personal space. This is your personal copy, you can
clone, branch and push things back here. If you have a branch or some
code you want to merge back in the main branch you do a pull request.
This is the way for external people to commit code back to PEcAn and
BETY. The pull request will start a review process that will eventually
result in the code being merged into the main copy of the codebase.

See
[https://help.github.com/articles/fork-a-repo](https://help.github.com/articles/fork-a-repo)
for more information, especially on how to keep your fork up to date
with respect to the original.

GIT Workflow
------------

GIT encourage to branch often
* Branch before working on feature
* One branch per feature
* You can switch easy between branches
* Merge feature into main line when branch done

    git branch <name of branch>
    git checkout <name of branch>
    repeat 
      write some code
      commit
    until done

    git checkout master
    git merge <name of brach>
    git push

If during above process you want to work on something else, commit all
your code, create a new branch, and work on new branch. As said before,
each feature should be in it’s own branch (for example each redmine
issue is a branch, names of branches are often the issue in a bug
tracking system).

Once you are all done with a branch you can delete it using git branch
-d <name of branch>

GitHub notes:
-------------

Easiest way to get working with GitHub is by installing the GitHub
client. More instructions for your specific OS and download of the
GitHub client, see
[https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git).
This will help you setup an SSH key to push code back to GitHub. To
checkout a project you do not need to have an ssh key and you can use
the https or git url to check out the code.

Working with rstudio
--------------------

1.  create account on github
2.  fork from
    [https://www.github.com/pecanproject/pecan](https://www.github.com/pecanproject/pecan)
3.  install Rstudio (www.rstudio.com)
4.  click (upper right) project
 *   create project
 *   version control
 *   Git - clone a project from a Git Repository
 *   paste repository url
        [https://www.github.com/](https://www.github.com/)<username>/pecan
 *   choose working dir. for repo

References:
-----------

[1] Scott Chacon, ‘Pro Git book’,
[http://git-scm.com/book](http://git-scm.com/book)
[2] GitHub help pages,
[https://help.github.com](https://help.github.com)/
[3] Main GIT page,
[http://git-scm.com/documentation](http://git-scm.com/documentation)
[4] Information about branches,
[http://nvie.com/posts/a-successful-git-branching-model](http://nvie.com/posts/a-successful-git-branching-model)/
[5] Another set of pages about branching,
[http://sandofsky.com/blog/git-workflow.html](http://sandofsky.com/blog/git-workflow.html)
