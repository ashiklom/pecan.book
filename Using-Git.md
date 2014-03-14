GIT and GitHub Overview
============

## GitHub

See our documentation for [reporting issues on Redmine and GitHub](https://help.github.com/articles/notifications)

## Git

Git is a version control software; GitHub is a project-hosting website that is similar to [Redmine](https://ebi-forecast.igb.illinois.edu/redmine/) but easier to use for open and collaborative development.

Git is a free & open source, distributed version control system designed
to handle everything from small to very large projects with speed and
efficiency. Every Git clone is a full-fledged repository with complete
history and full revision tracking capabilities, not dependent on
network access or a central server. Branching and merging are fast and
easy to do.

There are two fun tutorials for learnign git: [LearnGitBranching](http://pcottle.github.com/learnGitBranching/) and
[TryGit](http://try.github.com). More information is available in the [[References | Using-Git#references]]

In the rest of the document I will talk about URL’s to clone the code.
There a few URL’s you can use to clone a project, using https, ssh and
git. You can use either https or git to clone a repository and write to
it. The git protocol is read-only.

#### PEcAnProject GitHub location: https://github.com/organizations/PecanProject
* PEcAn source code: 
 * https://github.com/PecanProject/pecan.git
 * git@github.com:PecanProject/pecan.git
* BETYdb source code:
 * https://github.com/PecanProject/bety.git
 * git@github.com:PecanProject/bety.git

Recommended Workflow for PEcAn and BETY developers
--------------------------------------------------

**Each feature should be in it’s own branch** (for example each redmine
issue is a branch, names of branches are often the issue in a bug
tracking system).

**Commit and Push Frequency** On your branch, commit **_at minimum once a day before you push changes:_** even better: every time you reach a stopping point and move to a new issue. best: any time that you have done work that you do not want to re-do. Remember, pushing changes to your branch is like saving a draft. Submit a pull request when you are done.

### Before any work is done:

1. First fork pecan on github into your own github space ([github help: "fork a repo"](https://help.github.com/articles/fork-a-repo)) This allows you to create your own
copy of the code. When you do the fork a copy of the code is created and
placed in your personal space. This is your personal copy, you can
clone, branch and push things back here. If you have a branch or some
code you want to merge back in the main branch you do a pull request.
This is the way for external people to commit code back to PEcAn and
BETY. The pull request will start a review process that will eventually
result in the code being merged into the main copy of the codebase. See https://help.github.com/articles/fork-a-repo for more information, 
especially on how to keep your fork up to date with respect to the original.
1. Introduce yourself to GIT

        git config --global user.name "FULLNAME"
        git config --global user.email you@yourdomain.example.com

3. Clone to your local machine 
```bash 
git clone git@github.com:<username>/pecan.git
```
3. Define upstream 
```bash 
git remote add upstream git@github.com:PecanProject/pecan.git
```

### During development:

## Option 1: Always work in the same local branch

1. Get the latest code from the main repository
```bash 
git pull upstream master
```

2. Do some coding

3. Commit after each chunk of code (multiple times a day)
```bash
git commit -m "<some descriptive information about what was done>"
```

4. Push to YOUR Github (when a feature is working, a set of bugs are fixed, or you need to share progress with others)
```bash
git push origin <branchname>
```

4. Before submitting code back to the main repository, make sure that code compiles
```bash
./scripts/build.sh -c
```

5. submit pull request ([see github documentation](https://help.github.com/articles/using-pull-requests))




## Option 2: A new branch for each change

1. Make sure you start in master 
```bash
git checkout master
```
2. Make sure master is up to date 
```bash 
git pull upstream master
```
2. Build most recent versions of R packages ([`./scripts/build.sh -h` for help)](Installing-PEcAn#update-build-and-check-pecan))
```bash
./scripts/build.sh
```
3. Create a branch 
```bash
git checkout -b <branchname>
```
4. work/commit/etc 
```bash
git commit -m "<some descriptive information about what was done>"
```
* commit often; 
* each commit can address 0 or 1 issue, many commits can reference an issue ([see](#link-commits-to-issues))
* ensure that all tests are passing before anything is pushed into master.
5. make sure that code compiles
```bash
./scripts/build.sh -c
```
5. Push this branch to your github space 
```bash
git push origin <branchname>
```
6. submit pull request ([see github documentation](https://help.github.com/articles/using-pull-requests))

### After pull request is merged
1. Make sure you start in master 
```bash
git checkout master
```
2. delete branch remotely 
```bash
git push origin --delete <branchname>
```
3. delete branch locally 
```bash
git branch -D <branchname>
```


### Link commits to issues

When you work on or close an issue from a commit message, include the following text:

* [**Github**](https://github.com/blog/1386-closing-issues-via-commit-messages)
 * to close: "closes gh-xxx" (or syn. close, closed, fixes, fix, fixed)  
 * to reference: just the issue number (e.g. "gh-xxx")
 * avoid "closes #xxx" which will cross-reference Redmine issues 
* **Redmine**: 
 * to close: "fixes redmine #xxx" (or closes etc.) 
 * to reference: "redmine #xxx"
* **Bitbucket**: 
 * to close: reference and use web interface!
 * to reference: "re #xxx"


## For PEcAn 

```
./scripts/build.sh -c
```

### Committing Changes Using Pull Requests

GitHub provides a useful overview of how to submit changes to a project, [Using Pull Requests](https://help.github.com/articles/using-pull-requests).

Once you have added a feature on your local fork of the project that you would like to contribute, these are the steps:

* Submit a Pull Request
* Pull request is reviewed by a member of the PEcAn core group
* Any comments should be addressed
* Additional commits are added to the pull request
* When ready, changes are merged



Quick Git Overview:
-------------------

### GitHub notes:


Easiest way to get working with GitHub is by installing the GitHub
client. More instructions for your specific OS and download of the
GitHub client, see https://help.github.com/articles/set-up-git.
This will help you setup an SSH key to push code back to GitHub. To
checkout a project you do not need to have an ssh key and you can use
the https or git url to check out the code.

### Workflow

1. Create a Fork 
2. Clone a Repo
3. Make Changes
4. Commit
5. Submitting a Pull Request


The Basic Workflow is a starting point. The standard method used for PEcAn is found above under [Recommended Workflow](https://github.com/PecanProject/pecan/wiki/_preview#recommended-workflow-for-pecan-and-bety-developers)

* GIT encourages to branch "early and often"
 * First pull from master 
 * Branch before working on feature
 * One branch per feature
 * You can switch easily between branches
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
your code, create a new branch, and work on new branch. 



### Other Useful Git Commands:

* Delete a branch: `git branch -d <name of branch>`
* To push a branch git: `push -u origin `<name of branch>`
* To checkout a branch: 
  ```
  git fetch origin
  git checkout --track origin/<name of branch>
  ```


* Get the remote repository locally:

        git clone URL

* To update local repository to latest:

        git pull

* To add new files to to local repository:

        git add <file>

* To commit changes

        git commit <file|folder>

* To update remote repository:

        git push

* Show graph of commits:

        git log --graph --oneline --all

### Tags

Git supports two types of tags: lightweight and annotated. For more information see the [Tagging Chapter in the Git documentation](http://git-scm.com/book/ch2-6.html).

Lightweight are useful, but here we discuss the annotated tags that are used for marking stable versions, major releases, and versions associated with published results. 

The basic command is `git tag`. The `-a` flag means 'annotated' and `-m` is used before a message.  Here is an example:

    git tag -a v0.6 -m "stable version with foo and bar features, used in the foobar publication by Bob"

Adding a tag to the master branch must be done explicitly with a push, e.g.

    git push v0.6
    
To use a tagged version, just checkout: 

    git checkout v0.6
    
To tag an earlier commit, just append the commit SHA to the command, e.g. 

    git tag -a v0.99 -m "last version before 1.0" 9fceb02






## Git + Rstudio


Rstudio is nicely integrated with many development tools, including git and GitHub. 
It is quite easy to checkout source code from within the Rstudio program or browser.
The Rstudio documentation includes useful overviews of [version control](http://www.rstudio.com/ide/docs/version_control/overview) and [R package development](http://www.rstudio.com/ide/docs/packages/overview). 

Once you have git installed on your computer (see the [Rstudio version control](http://www.rstudio.com/ide/docs/version_control/overview) documentation for instructions), you can use the following steps to install the PEcAn source code in Rstudio.

#### For "read-only" version:

1.  install Rstudio (www.rstudio.com)
2.  click (upper right) project
 *   create project
 *   version control
 *   Git - clone a project from a Git Repository
 *   paste https://www.github.com/PecanProject/pecan
 *   choose working dir. for repo

#### For development:

1.  create account on github
2.  create a fork of the PEcAn repository to your own account https://www.github.com/pecanproject/pecan
3.  install Rstudio (www.rstudio.com)
4. generate an ssh key 
 * in Rstudio: 
  * `Tools -> Options -> Git/SVN -> "create RSA key"`
  * `View public key -> ctrl+C to copy`
 * in GitHub
  * go to [ssh settings](https://github.com/settings/ssh)
  * `-> 'add ssh key' -> ctrl+V to paste -> 'add key'` 
2. Create project in Rstudio
 *  `project (upper right) -> create project -> version control -> Git - clone a project from a Git Repository`
 * paste repository url `git@github.com:<username>/pecan.git>`
 * choose working dir. for repository

References:
-----------


#### Git Documentation

* Scott Chacon, ‘Pro Git book’,
[http://git-scm.com/book](http://git-scm.com/book)
* GitHub help pages,
[https://help.github.com](https://help.github.com)/
* Main GIT page,
[http://git-scm.com/documentation](http://git-scm.com/documentation)
* Information about branches,
[http://nvie.com/posts/a-successful-git-branching-model](http://nvie.com/posts/a-successful-git-branching-model)/
* Another set of pages about branching,
[http://sandofsky.com/blog/git-workflow.html](http://sandofsky.com/blog/git-workflow.html)
* [Stackoverflow highest voted questions tagged "git"](http://stackoverflow.com/questions/tagged/git?sort=votes&pagesize=50)


#### GitHub Documentation

When in doubt, the first step is to click the "Help" button at the top of the page.

* [GitHub Flow](http://scottchacon.com/2011/08/31/github-flow.html) by
Scott Chacon (Git evangelist and Ruby developer working on GitHub.com)
* [GitHub FAQ]((https://help.github.com/)
* [Using Pull Requests](https://help.github.com/articles/using-pull-requests)
* [SSH Keys](https://help.github.com/articles/generating-ssh-keys)