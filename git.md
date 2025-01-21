# Version Control

## Why Version Control?
* allows you to back up, modify and revert changes without manually making and managing	backup files

## Types of Version Control
* formal vs impromptu
* scalable vs too much work
* centralized vs decentralized
* concurrent vs locking
* diffs/patches vs snapshots

## Distributed vs Centralized
* centralized advantages:
	- it is simple -- everyone puts their code in the same place
	- works well for backup, undo and synchronization
	- backups are clear -- if you lose your data, all revisions are on the main server
* centralized disadvantages:
	- does not support merging and branching
	- check ins are slow
	- requires an 'always running' server to control and manage the main
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

## Distributed Version Control
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


# So You've Decided To Use Git

## Cheat Sheet
```bash
# REPOS AND BRANCHING
man git-<command>				# man for command
git help foo					# get help for git command foo
git init						# initialize new repository
git clone <repo address>		# fetch a repository you don’t yet have from remote
git remote -v					# lists remote origins your repo knows about
git remote show origin			# show repo’s url
git remote set-url origin <url>	# set url (esp if switching between ssh and https)
git checkout					# switch between branches you already have
git checkout -					# check out the last branch you were on
git checkout -b foo <branch>	# make a new branch called foo <from branch> and switch to it
git co <branch>					# when there exists a remote `origin/<branch>`, check out a local copy and set to track.
git co --track origin/<branch>	# check out remote branch foo and set your local to track to it - alternative to just `git co <branch>` if you have multiple remotes of the same name
git branch [-a]					# view branch info (-a: all repos, not just local)
git br -avv						# view all branches and the remote they are tracking to
git branch -r					# view remote branches
git branch -d foo				# delete branch that you’re done with
git branch -D foo				# force remove w/out merging
git status						# show current state of your repo

# COMMITS
git rev-parse HEAD				# show hash of HEAD (or whatever branch)
git merge-base foo bar			# get the last common commit between two branches
git show <hash>					# take a look at the changes in the commit before cherry-picking, etc

# WORKING WITH AN EXISTING REPO
git fetch						# get other people’s checked-in changes w/out merging into yours
git pull						# fetch + merge
git push						# push all committed files to the remote repository
git push -u origin main			# push and set tracking info for your branch to push/pull from origin
git push origin --delete foo	# remove branch foo from remote if it has been pushed to origin
git push --force --verbose --dry-run

# MERGING & REBASING
git merge foo					# merge foo into your current branch
git merge --squash foo			# merge foo into your current branch with one commit
git merge --no-ff foo			# merge foo into your current branch with one commit but retain history
git cherry-pick <hash>			# take changes in the hash commit and apply to the current branch - generates a new commit for the current branch
git cherry-pick A^..B			# cherry pick a range of commits from and including A to B
git rebase main					# pull changes from main into current branch and replay branch commits on top
git rebase --skip main			# avoid conflicts between your own commits as they are being applied

# WORKING WITH FILES
git add foo						# add foo to files to be committed
git add .						# stage all files in current dir and subdirs for commit
git add -A						# add entire working branch to stage
git add -p -- foo.txt			# interactively add just certain lines of a filename to the stage
git co bar.txt					# checkout file from HEAD, discarding changes
git co main bar.txt				# checkout file from another branch, overwriting curr branch changes
git co HEAD -- */*.config		# check out everything matching filename foo from HEAD
git reset						# reset HEAD to specified state (unstage changes). does not alter files
git reset --hard main			# reset HEAD and make files identical to main
git clean						# remove untracked files from the current working dir
git clean -n					# show which files would be removed
git clean -f					# remove untracked files (not dirs)
git rm foo						# remove local and remove remote on next push
git rm --cached foo.txt			# retain local and remove remote on next push - this will delete foo.txt for others who pull

# VIEWING HISTORY
git show -B -w <hash>								# show changes that were made for a commit, ignoring whitespace
git show HEAD~:./dir/file							# show output of the file in dir/ as of last commit before HEAD
git log --follow foo.txt							# see commit history of file
git log --follow -p foo.txt							# see commit history and patch diff (code changes) of file
git log --first-parent main							# see commits made to main
git log --author="Alex"								# see commits made by Alex (regex can be used for the name)
git log --first-parent								# see commits for current branch and then parent and its commits
git grep -I <pattern>								# search files for pattern (-I excludes binaries)
git grep <pattern> -- *.h *.cpp						# grep through only .h and .cpp files
git diff											# show all changes made (but not necessarily added)
git diff --cached									# show diff of staged, uncommitted changes
git diff main -- foo.txt							# diff between your file (staged or not) and the version in main
git diff --name-status A..B							# name and status of files that differ between branch A and head of B
git diff --name-status A...B						# name and status of files that differ between branches A and B since they diverged
git diff main..										# view diff between main and current unstaged changes
git diff -B -w --shortstat A..B						# just list num of files changed, insertions and deletions between commits
git rev-list --all | xargs git grep <expression>								search history for an expression
git log -S 'Needle' -p									another way to search history to see when a string was added or removed, with patch
git log -S 'Needle' --source --all								search all branches
																										or search with regex:
git log -G "^(\s)*function foo[(][)](\s)*{$" --source --all		search all branches with regex

git commit                      prep files to be committed
git commit foo                  revert to commit foo

# RESET RENAMELIMIT:
git config merge.renameLimit 999999
git config --unset merge.renameLimit

# git pull is unable to resolve references: error: Ref refs/remotes/origin/{branch} is at <hash1> but expected <hash2>
git gc --prune=now              clean up and optimize local repo, removing loose objects of any age (now)
git remote prune origin         delete all stale remote tracking branches under origin (remove local branches that are no longer in remote origin)
# actually try this:
git update-ref -d refs/remotes/origin/<branch_thats_barfing>

# CLEAN UP FILES
git clean -n -d                 see which files/dirs would be removed
git clean -f -d                 remove all untracked files that are not in .gitignore, including dirs

# REMOVE IGNORED FILES (reset to a "just cloned" state)
git status --ignored			# see what's ignored
git clean -ndx					# dry run, include directories, uncommitted and ignored files
git clean -ndX					# dry run, include only ignored directories and files (leave uncommitted)
git clean -fdX/x				# do the removal (with force)
git status --ignored			# check your work

# BUNDLE BUNDLE WHO'S YOUR BUNDLE?
# create:
git bundle create my-repo.bundle HEAD main
# extract:
git clone my-repo.bundle <optional dir>

```

## Git Terms
* a **repository** is a body of code and its version history data, including all files, branches and their commits, and tags
* **staging**: lining up changes to upload to the repository
* **commit**: a snapshot of changes, author, date, committer, parent commit
* **author** and **committer** are differentiated
* a **remote** is a mirror of a **local** repo that is universally accessible to all users
* **branch**: a parallel path of development starting from a commit that's in the tree
* **HEAD** is a pointer to the last commit in a branch
* **tag**: a ref that points to a specific point in history
	- unlike a branch, a tag never changes
	- tags are often used to mark releases
```sh
	git tag -l 'foo'  # list all tags with 'foo'
```
* many git commands can be performed on either a branch or a file or files. Using the `--` notation explicitly tells git "hey, this is a file!" in case you have a file and a repo with the same name

### Anatomy of a Commit
* **SHA1**: a hash standard that takes various meta data (commit message, committer, commit date, author, authoring date, entire working directory) and generates a completely unique hash string


### Head vs Main vs Origin
* `HEAD` is an official notion in git, `HEAD` always has a well defined meaning. main and origin are common names usually used in git but they don't have to be.
* **HEAD:** the current commit your repo is on.
	- Most of the time HEAD points to the latest commit in your branch, but that doesn't have to be the case.
	- HEAD really just means "what is my repo currently pointing at"
	- In the event that the commit HEAD refers to is not the tip of any branch, this is called a "detached head".
* **main:** The name of the default branch that git creates for you when first creating a repo.
	- most shops have everyone pushing to main, and main is considered the definitive view of the repo
	- it's also common for release branches to be cut off of main for release
	- your local repo has its own main branch, that almost always follows the main of a remote repo (*when would it not?*)
* **origin:** The default name that git gives to your main remote repo
	- Your box has its own repo, and you most likely push out to some remote repo that you and all your coworkers push to.
	- That remote repo is almost always called origin, but it doesn't have to be.

## Authentication
* you can use either ssh or https to authenticate when cloning/pushing/pulling
* [swapping remote from ssh to https](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories?platform=mac#switching-remote-urls-from-ssh-to-https)

### SSH
* ssh is the most secure option and doesn't require you to enter your password every time 
* more tedious to set up -- requires generating and managing an SSH key
* [github doc on ssh](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
* there are additional steps to manage keychain for MacOS -- check [this guide](https://medium.com/codex/git-authentication-on-macos-setting-up-ssh-to-connect-to-your-github-account-d7f5df029320) except change `Host *` to `Host github.com` in your `~/.ssh/config` file

### HTTPS
* https is more convenient/user friendly and there are more options for how to handle it
* https requires one of two things every time you request a git operation:
	- your username and password
	- your Personal Access Token (PAT)
* sending this information every time is tedious, so there are ways to cache your credentials: using `osxkeychain` on mac or `wincred` on windows
* another convenient cross-platform option is [git credential manager](https://github.com/git-ecosystem/git-credential-manager) -- a tool that can be used to manage credentials and integrates with many other systems like AWS and Azure


## Git Configuration
* typically git can be configured at a system, user or repo level. the most common configuration is at the user level. 
* to view your configuration:
```sh
	git config --list --system		# show only system-wide configuration
	git config --list --global		# show only user configuration
	git config --list --local		# show only locally configured values
	git config --list				# show all configured values: system, global and local for current repo, if any
	git config --list --show-origin	# show all configured values and their origin
	git config --get user.name		# show effective value for a given config property
```

### .gitconfig
* **TODO**

### .gitignore
* frequently there will be files in your git folder that you don’t want on the interweb, or that are unnecessary/inapplicable to the repo.
* use .gitignore to tell git which files to leave alone
* don’t gitignore .gitignore -- it will automatically prevent other contributors to the repo from polluting the repo with the same files you’re omitting
* Note that ignores that you don't want to share, e.g. specific configuration files for an IDE that only you use, can be ignored using .git/info/exclude. The format of this file is the same as .gitignore, but this file will never be committed
* to exclude everything in a directory *except* some file or files:
```sh
	# in .gitignore, excludes everything in my_secret_directory except public files
	my_secret_directory/
	!my_secret_directory/publicfile.html
```
* git will not commit empty directories, so if the directories need to be there for some reason, put an empty file or something so they can be committed
* stuff you should gitignore:
	- sensitive information
	- node_modules/
* don't gitignore:
	- gitignore
	- package.json-lock


### .gitattributes
* `.gitattributes` can be configured to contain some useful instructions about how to treat your version-controlled content
* `export-ignore` tells git to ignore certain files/folders when someone downloads your repository
* `text-auto` normalizes line-endings to use LF
* `eol` specify line ending (say, for just one file or filename pattern)
* `binary`/`text` specifies how to treat files when commands like `git co`, `git add`, `git commit` and `git merge` are run
		- `text` enables end-of-line normalization
* `merge` strategy: specify default merge strategies for different files
* `-diff` do not merge these files
```sh
	# put it all together:
	src/foo.php text eol=crlf       # treat foo.php as text and make sure it has 'crlf' line endings
	*.jpg binary                    # always treat jpg files as binary
	package-lock.json merge=ours    # use ours when merge conflicts arrive
	# do not try to merge these files:
	yarn.lock -diff
	public/build/js/*.js -diff
	public/build/css/*.css -diff
```

## Basic Operations

### Merging vs Rebasing
* **merge**: combining branches together
	- `git checkout my-cool-new-feature`: get the branch you want
	- `git merge main`: merge in main -- most recent commit on `my-cool-new-feature` will now be the merge of main
	- there should rarely be conflicts -- maybe only if two people are adding at the same time
* **rebase**: changing history
	- `git rebase main` from your local branch basically says "reset to main and then replay all my work on top of that"
	- great for local development that no one else is working on but can create issues when sharing a branch

### Playing Well With Others
* change history locally, never globally
* never force push unless you have to or are working on a branch you know is not being used by others
	* if git suggests that you `git push -f (force push)` DON'T. reconsider what you've done.
* if you are asked to force pull, there could be something malicious going on
* focused commits with clear messages
* follow project standards for branching, tagging, etc

### Stashing
* git will not let you switch branches if you have changes in the branch you're switching to that could override your current work
* to get around this, we use `stashing`

```bash
git stash
git co f3
# do work on f3
git co f4
git stash pop
```

```
git stash list                  view all items in the stash
git stash                       stash all your current changes
git stash -u                    include untracked files
git stash push <file>           stash only one file
git stash push -u -m "message"  manually push everything onto the stack with message
git stash push -m "cool!" ./Program.cs  stash just one file
git stash pop                   remove most recent stash item and apply it to the branch
git stash pop <file>            remove <file> from stash and apply it to the branch
git stash pop 2                 pop the item in index 2
git stash apply <file>          apply <file> to the branch w/out removing it from the stash
git stash show <stashname>      show summary of stash w/out popping
git stash show stash@{1} -p     show patch of second item in stash
git stash show -p               show full patch diff of most recent stash
git stash drop <stashname>      w/out stashname, drops most recent
git stash clear                 CAREFUL! this will delete your reflog as well
```
* you might have changes in your stash that would overwrite your uncommitted work, but maybe that's what you want. to apply the stash and overwrite any merge conflicts with what is in the stash, try:
		`git checkout stash -- .`


## Advanced Operations

### Amend A Commit
* so say you're doing some work and you realize that you need to make a change to your last commit (for example, you forgot to save a file you wanted to commit)
```bash
	# you can reset and re-commit:
	git reset HEAD~
	git aa
	git cm
	# or you can alter the commit:
	git aa
	git commit --amend
	# alter commit message if needed, save quit
```

### Selective Merging
* say you have a bunch of commits on a feature branch (`f3`) that are for three different issues. You want to group these changes into three commits when you merge into `main` so you can properly document them. how to do?

```bash
git co f3
git rebase main				# make sure feature is up to date - useful for diffing later
git co hotfix				# check out some intermediary branch
git reset --hard main		# make sure it's the same as main
git cherry-pick <SHA1>		# cherry pick all your commits for one issue onto hotfix from f3 in the order you want to apply them
git reset HEAD~n			# where n is the # of commits, unstages all those cherrypicks
git add -A					# you may need to do this if there is a new file
git commit -a				# write your lovely message for the one commit
git co main
git merge hotfix
# repeat for all issues until all commits in f3 have been merged into main
git diff f3..hotfix			# make sure you didn't forget anything
git push
```

### "Reset" One Branch To Another
* when working locally, you can rewrite history and hard reset your feature to another
* however, you should never do this on shared branches -- for instance, if you have discrepencies between `main` and `staging` and you need to make `staging` just like `main`, how do you do that without rewriting `staging`'s history?
* [SO to the rescue](https://stackoverflow.com/a/54889409) -- basically you're going to start on the branch you want to keep the changes from (`main`) and use that as a base to recreate the branch you want to reset (`staging`)
```bash
# first check out main in a detached state so you can mess around with it
git co --detach main		
# this is where the magic happens ✨ use the 'ours' merge strategy to merge staging into main, keeping all changes from main							
git merge -s ours staging -m "reset staging to main"
# recreate the staging branch from your current commit history
git branch -f staging
# push your changes to make them take effect -- you should not need to force bc this branch should have the same commit history as remote, plus your new "reset" commit
git push origin staging
```


### Fixup
* sometimes you may need to fix an old commit. `fixup` followed by `autosquash` can help (you can also use `--squash` which will allow you to edit the commit message)
* note that this should **not** be performed on commits that have already been merged/pushed for other devs to modify. this should only be performed on code strictly under your own control
* say you have a commit for feature A and then make a commit for feature B, but then you find another modification you need to make for feature A:
```bash
	# Make commits for features A and B:
	$ git add featureA
	$ git commit -m "Feature A is done"
	[dev fb2f677] Feature A is done
	$ git add featureB
	$ git commit -m "Feature B is done"
	[dev 733e2ff] Feature B is done
	# oops, not here's your shameful commit to fix A:
	$ git add featureA
	$ git commit --fixup fb2f677
	[dev c5069d5] fixup! Feature A is done
	# now the log shows three commits:
	c5069d5 fixup! Feature A is done
	733e2ff Feature B is done
	fb2f677 Feature A is done
	ac5db87 Previous commit
	# When you're done making fixups:
	$ git rebase -i --autosquash ac5db87
	pick fb2f677 Feature A is done
	fixup c5069d5 fixup! Feature A is done
	pick 733e2ff Feature B is done
	# That ^ will have opened up your editor. Just save and close and now:
	$ git log --oneline
	ff4de2a Feature B is done
	5478cee Feature A is done       # contains fixup
	ac5db87 Previous commit
```
* [read more info about fixup](https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html)

### Omit A File From A Commit
* use `update-index` to preserve local changes to something like a config file that points to your sandbox db -- you have changes that are just for you that you don't want to push
```sh
	# ignore changes to this file for now:
	git update-index --assume-unchanged foo.ts
	# once you're done with the file and want to reassimilate it into version control:
	git update-index --no-assume-unchanged foo.ts
```

### Recovering Lost Work With Reflog
* the reflog contains a history of all recent changes
* say you did a `git reset --hard` but now you need those changes -- `reflog` has your back
* using `reflog` you can view commits that are not visible with `git log`
* suppose you were doing some work on `f1`, switched to main but then for some reason your commits on `f1` are gone!
```sh
	# viewing the reflog, we can see missing commits foo, bar and baz
	$ git reflog
	cf42fa2... HEAD@{84}: checkout: moving to main
	73b9363... HEAD@{85}: commit: baz
	547cc1b... HEAD@{86}: commit: bar
	1dc3298... HEAD@{87}: commit: foo
	26fbb9c... HEAD@{89}: checkout: moving to f1
	# reset your f1 branch to the state of the most recent commit, baz:
	$ git co f1
	$ git reset --hard 73b9363
	# or you can grab a specific file from a commit in the reflog:
	$ git co f1
	$ git co 73b9363 -- <filename>
```
* caveats:
	- `reflog` won't save your work forever -- eventually "unreachable" work gets cleaned up
	- `reflog` only works with changes that were committed -- sorry, whatever you just lost with `git stash drop` is gone forever 
	- `reflog` is yours and yours alone -- it can't help you with another person's uncommitted, unpushed work


### Removing Files From Git
* remove a local file from the directory
```sh
	git rm foo
	git commit -m "removed foo"
```
* remove the file only from the Git repository and not remove it from the filesystem:
```sh
	git rm --cached foo
	git commit -m "removed foo"		# only need to do this if the file was pushed
```

### Changing Master to Main
* [see this step-by-step process](https://gist.github.com/danieldogeanu/739f88ea5312aaa23180e162e3ae89ab)

### Cleansing Sensitive Information
* use bfg repo cleaner: https://rtyley.github.io/bfg-repo-cleaner/
* steps:
	- clone repo using `--mirror`:
	- make changes to the repo
	- cd into repo, call reflog, gc and push
	```
		git clone --mirror git://example.com/my-repo.git
		java -jar bfg.jar --delete-folders .git --delete-files .git  --no-blob-protection  my-repo.git
		cd my-repo.git
		git reflog expire --expire=now --all && git gc --prune=now --aggressive
		git push
	```

### Formatting
* you can use various options to format data about commits and files

#### Dates
* format options include:
		%cd: committer date (format respects --date= option)
		%cD: committer date, RFC2822 style
		%cr: committer date, relative
		%ct: committer date, UNIX timestamp
		%ci: committer date, ISO 8601-like format
		%cI: committer date, strict ISO 8601 format
* use `--format="%cd" --date=short` to easily get YYYY-MM-DD


## Git Submodules
* [github blog post on submodules](https://github.blog/2016-02-01-working-with-submodules/)
* submodules are a way of nested interdependent repositories together, usually with shared configurations and dependencies managed in a top-level repository
* submodules can be really handy for local development and testing where multiple services need to be built and deployed together

### Adding Submodules
* submodules are tracked in the `.gitmodules` file of the parent repo
* to add a submodule to a repo, use `git submodule add <repo url> <path>`. this will clone the submodule and add or update the `.gitmodules` file
* commit the submodule by committing the `.gitmodules` file and the submodule directory

### Updating Submodules
* when you make a change to a submodule, the change is tracked in the submodule's revision history, but the parent is aware that the submodule has changed
* suppose I make a change to a submodule - I need to also commit the submodule in the parent and push it for another dev to get my changes

### Submodule-specific Commands
```sh
# clone a repo and all its submodules
git clone git@github.com:foo/bar.git --recurse-submodules

# create a new repo -- this is our parent
mkdir my-parent-repo && cd my-parent-repo && git init
# add a submodule to existing repo
git submodule add https://bitbucket.org/jaredw/awesomelibrary
# commit the submodule
git add .gitmodules awesomelibrary/ && git commit -m "added submodule"

# by default, submodule folders are empty when a parent is cloned. fetch the files with:
#	- this will fetch the submodule at the commmit that is checked in to the parent repo, not necessarily latest
#	- --recursive will fetch all the submodule's submodules, if any
git submodule update --init [--recursive]
# set the submodule to it's latest commit, regardless of what's checked into the parent
git submodule update --remote [--recursive]

```

## Gotchas
* git might not see casing changes on files as an actual change, so you might need to explicitly "move" the file
```
	git mv duck.ts Duck.ts
```

# Beyond Just Git

## Github
* github is a less distributed paradigm of git
* github has more specific parameters (forking, for example)
* gh has more rules about who to trust
* forking:
	- creating a parallel (possibly divergent) copy of a persons repo
	- creates a 'center' of the hub, implying that the others are not the center
* other cool features:
	- wiki
	- gist: paste bin with a little bit of version control
	- issue trackers
	- cool graphs
	- repo descriptions and automatic README display

## Git Credential Manager
* [git credential manager](https://github.com/git-ecosystem/git-credential-manager/tree/main)
* if you're using https, the simplest way is to use GCM
* supported by github, bitbucket and azure devops

## Big Files
* if you need to include large assets in your repo, it's bad -- it slows down your git operations and takes up space with information that will never change
* use [git-lfs](https://git-lfs.com/) to replace large files with text pointers to the actual file which is hosted on a remote server

