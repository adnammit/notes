 # VERSION CONTROL
---
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
man git-<command>               man for command
git clone <repository address>  fetch a repository you don’t yet have from remote
git checkout                    switch between branches you already have
git checkout -b foo <branch>    make a new branch called foo <from branch> and switch to it
git checkout -- foo             restore foo, which you accidentally deleted
git checkout foo.txt            checkout foo.txt, discarding current changes
git branch [-a]                 view branch info (-a: all repos, not just local)
git branch -d foo               delete branch foo (say you’re done with foo, it’s merged back into master)
git branch -D foo               force remove w/out merging
git push origin --delete foo    remove branch foo from remote if it has been pushed to origin
git init                        initialize new project
git fetch                       get other people’s checked-in changes w/out merging into yours
git merge                       combine one branch with another
git pull		                fetch + merge
git status                      show current state of your repo
git add foo                     add foo to files to be committed
git add .                       stage all files in current dir and subdirs for commit
git add -A                      add entire working branch to stage
git reset                       clear everything you added
git rm foo                      remove local and remote
git rm --cached foo             remove foo from remote
git log                         see previous commits
git log --follow foo            see history of foo
git grep -I <pattern>           search files for pattern (-I excludes binaries)
git grep <pattern> -- *.h *.cpp grep through only .h and .cpp files
git diff                        show all changes made (but not necessarily added)
git diff --cached               show what is about to be committed
git diff --name-status A..B     name and status of files that differ between branch A and head of B
git diff --name-status A...B    name and status of files that differ between branches A and B since they diverged
git commit                      prep files to be committed
git commit foo                  revert to commit foo
git push                        push all committed files to the repository
git push --force --verbose --dry-run
git push -u origin <branch>     push your branch to remote and set tracking info for your branch to push/pull from origin
git remote -v                   lists remote origins your repo knows about
git remote show origin          show repo’s url
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
```
git stash list
git stash pop <name>
git stash show <stashname>    show stash <name> w/out popping
git stash show -p             show most recent stash
```

## PL GIT
* we use many custom tools for git -- they’re located in `/usr/local/lib/pl_tools/bin`
* update tools with `git tools update`
* to roll out `__web__` use `roll_out site`
* delete a branch with `git branch -d <name>`
    - this has extra goodness in it to remove the repo site as well
* view a file from another git branch in emacs with `C-x v ~`, then give br name or SHA


## USING GIT
export GIT_EDITOR=vim (or whatever editor you’d like) in .bashrc
the repository is a snapshot of your projects
git init initializes a new project
git show: look at a repo
man git-<command>
rm -rf .git -- nuke the history
stage change: lining up changes to upload to the repository
touch foo  -and then-  git add foo
making a commit:
snapshot of changes, author, date, committer, parent commit
author and committer are differentiated
git commit || man git-commit
git revert <commit to revert to>
A snapshot is changes made to a file plus pointers to unchanged files

* remote: is a clone of more or less the same repo
    - `git remote add <name> <url>`
* tag: marker attached to a specific
    - `git tag -l 'foo'  //list all tags with 'foo'`
* branch: a parallel path of development starting from a commit that's in the tree
* HEAD is the last commit in the currently checked-out branch

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
