# VERSION CONTROL

## WHY USE IT?

* allows you to back up, modify and revert changes without manually making and managing
  backup files


## TYPES OF VERSION CONTROL
* formal vs impromptu
* scalable vs too much work
* centralized vs decentralized
* concurrent vs locking
* diffs/patches vs snapshots


## DISTRIBUTED VS CENTRALIZED

* centralized advantages:
  - it is simple -- everyone puts their code in the same place
  - works well for backup, undo and synchronization
  - backups are clear -- if you lose your data, all revisions are on the master server
* centralized disadvantages:    
  - does not support merging and branching
  - check ins are slow
  - requires an 'always running' server to control and manage the master
* distributed advantages:
  - focus is on sharing changes (every change has a guid)
  - design relies on merging and branching, so merging multiple users' work is easy
  - localized:
      * each user has their own sandbox
	  * can work offline
      * diffs, commits and reverts are all local and fast
  - less management -- there's no 'always running' server software to install
* distributed disadvantages:
  - if you lose your work, there's no guaranteed backup (unless using something like github?)
  - there's no 'latest version' so it's not really clear which member of the team is sitting
    on the most recent changes
    * however you can create a central location to clarify this
  - there aren't really revision numbers, though changes have guid's
    * you can tag releases with names though



## DISTRIBUTED VERSION CONTROL
* goals
  - reliability
  - making sure you get the most recent version
  - eliminate network dependency
* what is git all about?
  - decentralized
	* peer share, rather than server downloadable
  - distributed
  - data insurance
  - porcelain vs plumbing
	* porcelain: issue a command and it just works
	* plumbing: a lot more work: the guts of it
  - free
* local operations:
    - working directory
    - staging area
    - git directory (repository)
* once something makes it to the repository, it's pretty much immutable


## SO YOU'VE DECIDED TO USE GIT:

### CHEAT SHEET
```
# Repos and Branching
man git-<command>               man for command
git init                        initialize new repository
git clone <repository address>  fetch a repository you don’t yet have from remote
git remote -v                   lists remote origins your repo knows about
git remote show origin          show repo’s url
git checkout                    switch between branches you already have
git checkout -b foo <branch>    make a new branch called foo <from branch> and switch to it
git checkout -- foo             restore foo, which you accidentally deleted
git checkout bar.txt            checkout file from branch remote, discarding local changes
git checkout master bar.txt     checkout file from another branch, overwriting curr branch changes
git branch [-a]                 view branch info (-a: all repos, not just local)
git branch -d foo               delete branch foo (say you’re done with foo, it’s merged back into master)
git branch -D foo               force remove w/out merging
git status                      show current state of your repo
git rev-parse HEAD              show hash of HEAD (or whatever branch)
git merge-base foo bar          get the last common commit between two branches

# Working With an Existing Repo
git fetch                       get other people’s checked-in changes w/out merging into yours
git pull		                fetch + merge
git push                        push all committed files to the remote repository
git push -u origin master       push and set tracking info for your branch to push/pull from origin
git push origin --delete foo    remove branch foo from remote if it has been pushed to origin
git push --force --verbose --dry-run

# Merging & Rebasing
git merge foo                   merge foo into your current branch
git merge --squash foo          merge foo into your current branch with one commit
git merge --no-ff foo           merge foo into your current branch with one commit but retain history
git cherry-pick <hash>          take changes in the hash commit and apply to the current branch.
                                generates a new commit for the current branch
git show <hash>                 take a look at the changes in the commit before cherry-picking, etc

# Working with Files
git add foo                     add foo to files to be committed
git add .                       stage all files in current dir and subdirs for commit
git add -A                      add entire working branch to stage
git add -p -- foo.txt           interactively add just certain lines of a filename to the stage
git reset                       reset HEAD to specified state (unstage changes). does not alter files
git reset --hard master         reset HEAD and make files identical to master
git clean                       remove untracked files from the working tree
git rm foo                      remove local and remote
git rm --cached foo.txt         remove foo from remote
git log                         see previous commits

# Viewing history
git log --follow foo.txt        see commit history of file
git log --follow -p foo.txt     see commit history and patch diff (code changes) of file
git grep -I <pattern>           search files for pattern (-I excludes binaries)
git grep <pattern> -- *.h *.cpp grep through only .h and .cpp files
git diff                        show all changes made (but not necessarily added)
git diff --cached               show diff of staged, uncommitted changes
git diff master -- foo.txt      diff between your file (staged or not) and the version in master
git diff --name-status A..B     name and status of files that differ between branch A and head of B
git diff --name-status A...B    name and status of files that differ between branches A and B since they diverged
git commit                      prep files to be committed
git commit foo                  revert to commit foo


# reset renameLimit:
git config merge.renameLimit 999999
git config --unset merge.renameLimit

```

### QUICK GUIDE
make a new repo:

```
git init
git remote add origin https://github.com/adnammit/yourRepo.git
git add .
git commit -m "initialized repo"
git push origin master
```
working with an existing repo:
```
git pull origin master
git add .
git commit -m "updates foo"
git push origin master
```

### STASHING
* git will not let you switch branches if you have changes in the branch you're switching to that could override your current work
* to get around this, we use `stashing`
```bash
    git stash
    git co f3
    //do work on f3
    git co f4
    git stash pop
```

```
git stash list
git stash                       stash all your current changes
git stash -u                    include untracked files
git stash push <file>           stash only one file
git stash push -u -m "message"  manually push everything onto the stack with message
git stash pop <file>            remove <file> from stash and apply it to the branch
git stash pop 2                 pop the item in index 2
git stash apply <file>          apply <file> to the branch w/out removing it from the stash
git stash show <stashname>      show stash <stashname> w/out popping
git stash show -p               show most recent stash
git stash drop <stashname>      w/out stashname, drops most recent
```

# PL GIT
* we use many custom tools for git -- they’re located in `/usr/local/lib/pl_tools/bin`
* update tools with `git tools update`
* to roll out `__web__` use `roll_out site`
* delete a branch with `git branch -d <name>`
    - this has extra goodness in it to remove the repo site as well


### EMACS:
* view a file from another git branch in emacs with `C-x v ~`, then give br name or SHA

### PL WORKFLOW:
```
$ git co f1

  // do some work in f1

$ git add -A
$ git commit -m "message"

  // do some more work, add/commit again as needed

$ git co master
$ git merge --squash f1   // or --no-ff
$ git commit
$ git push

  //reset branch for another purpose and roll out packages:

$ git co f1
$ git reset --hard master
  // get the last commit hash prior to the most recent reset:
LAST_COMMIT_BEFORE_RESET=git reflog show --pretty='format:%H %gs' | awk '/.* reset: moving to .*/{getline; print $1; exit;}'
roll_changes_pkgs --revs ${LAST_COMMIT_BEFORE_RESET} HEAD
```

### SET UP A WEBSITE:
```
$ git co -b f4                      // create a new branch
$ roll_out site
$ roll_out min_site                 // remember you can't have VS open to roll pl_app
$ roll_out <whatever pkgs>
$ ./build/copy_org perflogic        // do each TWICE, one at a time
```


## USING GIT
* staging: lining up changes to upload to the repository
* making a commit: a commit is a snapshot of changes, author, date, committer, parent commit
* author and committer are differentiated
* remote: is a clone of more or less the same repo
    - `git remote add <name> <url>`
* tag: marker attached to a specific
    - `git tag -l 'foo'  //list all tags with 'foo'`
* branch: a parallel path of development starting from a commit that's in the tree
* HEAD is a pointer to the last commit in the currently checked-out branch
* many git commands can be performed on either a branch or a file or files. Using the `--` notation explicitly tells git "hey, this is a file!" in case you have a file and a repo with the same name


### ANATOMY OF A COMMIT
* **SHA1**: a hash standard that takes various meta data (commit message, committer, commit date, author, authoring date, entire working directory) and generates a completely unique hash string


### HEAD VS MASTER VS ORIGIN
* HEAD is an official notion in git, HEAD always has a well defined meaning. master and origin are common names usually used in git but they don't have to be.

* **HEAD:** the current commit your repo is on.
    - Most of the time HEAD points to the latest commit in your branch, but that doesn't have to be the case.
    - HEAD really just means "what is my repo currently pointing at"
    - In the event that the commit HEAD refers to is not the tip of any branch, this is called a "detached head".

* **master:** The name of the default branch that git creates for you when first creating a repo.
    - In most cases, "master" means "the main branch"
    - Most shops have everyone pushing to master, and master is considered the definitive view of the repo. But it's also common for release branches to be made off of master for releasing.
    - Your local repo has its own master branch, that almost always follows the master of a remote repo.

* **origin:** The default name that git gives to your main remote repo
    - Your box has its own repo, and you most likely push out to some remote repo that you and all your coworkers push to.
    - That remote repo is almost always called origin, but it doesn't have to be.





### MERGING VS REBASING
* merge: combining branches together
    - git checkout mywork : get the branch you want
    - git merge master : merge in 'master'
    - there should rarely be conflicts -- maybe only if two people are adding at the same time
* rebase: changing history
    - this is mostly troublesome when you're rebasing something already pushed to others


### .GITIGNORE
* frequently there will be files in your git folder that you don’t want on the interweb, or that are unnecessary/inapplicable to the repo.
* use .gitignore to tell git which files to leave alone
* don’t gitignore .gitignore -- it will automatically prevent other contributors to the repo from polluting the repo with the same files you’re omitting
* Note that ignores that you don't want to share, e.g. specific configuration files for an IDE that only you use, can be ignored using .git/info/exclude. The format of this file is the same as .gitignore, but this file will never be committed


### REMOVING FILES FROM GIT
* remove a local file from the directory
 ```
    $ git rm foo		
    $ git commit -m “removed foo"
```
* remove the file only from the Git repository and not remove it from the filesystem:
```
    $ git rm --cached foo
    $ git commit -m "removed foo"   //only need to do this if the file was pushed
```


### CLEANSING SENSITIVE INFORMATION
* if you remove a file from the repo itself, sensitive info may still appear in the commit history
* add your files to .gitignore
* supposing the file has been pushed, remove from the repo with `$ git rm --cached my_file`
* scrub the file from past commits by finding the SHA-1 for the first commit of the file and then doing:
```
  $ git filter-branch -f --index-filter \ "git update-index --remove my_file" b8b53f2^..HEAD"
  $ git push --force --verbose --dry-run
  $ git push --force
```


## GITHUB
* github is a less distributed paradigm of git
* github has more specific parameters (forking, for example)
* gh has more rules about who to trust
* HTTP vs SSH clones:
    - use ssh clone: it uses the key in your account
* forking:
    - creating a parallel (possibly divergent) copy of a persons repo
    - creates a 'center' of the hub, implying that the others are not the center
* other cool features:
    - wiki
    - gist: paste bin with a little bit of version control
    - issue trackers
    - cool graphs
    - repo descriptions and automatic README display
* rules to play well with others:
    - change history locally, never globally
    - never force push unless you have to
	 * if git suggests that you `git push -f (force push)` DON'T. reconsider what you've done.
    - if you are asked to force pull, there could be something malicious going on.
 	 * communicating pushes/pulls is necessary
    - focused commits with clear messages
    - follow project standards for branching, tagging, etc
