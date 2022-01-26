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


# SO YOU'VE DECIDED TO USE GIT:

### CHEAT SHEET
```c
// REPOS AND BRANCHING
man git-<command>               man for command
git help foo                    get help for git command foo
git init                        initialize new repository
git clone <repo address>        fetch a repository you don’t yet have from remote
git remote -v                   lists remote origins your repo knows about
git remote show origin          show repo’s url
git remote set-url origin <url> set url (esp if switching between ssh and https)
git checkout                    switch between branches you already have
git checkout -                  check out the last branch you were on
git checkout -b foo <branch>    make a new branch called foo <from branch> and switch to it
git co <branch>                 when there exists a remote `origin/<branch>`, check out a local copy and set to track.
git co --track origin/<branch>  check out remote branch foo and set your local to track to it
																alternative to just `git co <branch>` if you have multiple remotes of the same name
git branch [-a]                 view branch info (-a: all repos, not just local)
git br -avv                     view all branches and the remote they are tracking to
git branch -r                   view remote branches
git branch -d foo               delete branch that you’re done with
git branch -D foo               force remove w/out merging
git status                      show current state of your repo

// COMMITS
git rev-parse HEAD              show hash of HEAD (or whatever branch)
git merge-base foo bar          get the last common commit between two branches
git show <hash>                 take a look at the changes in the commit before cherry-picking, etc

// WORKING WITH AN EXISTING REPO
git fetch                       get other people’s checked-in changes w/out merging into yours
git pull		                fetch + merge
git push                        push all committed files to the remote repository
git push -u origin master       push and set tracking info for your branch to push/pull from origin
git push origin --delete foo    remove branch foo from remote if it has been pushed to origin
git push --force --verbose --dry-run

// MERGING & REBASING
git merge foo                   merge foo into your current branch
git merge --squash foo          merge foo into your current branch with one commit
git merge --no-ff foo           merge foo into your current branch with one commit but retain history
git cherry-pick <hash>          take changes in the hash commit and apply to the current branch.
																generates a new commit for the current branch
git cherry-pick A^..B           cherry pick a range of commits from and including A to B
git rebase master               pull changes from master into current branch and replay branch commits on top
git rebase --skip master        avoid conflicts between your own commits as they are being applied

// WORKING WITH FILES
git add foo                     add foo to files to be committed
git add .                       stage all files in current dir and subdirs for commit
git add -A                      add entire working branch to stage
git add -p -- foo.txt           interactively add just certain lines of a filename to the stage
git co bar.txt                  checkout file from HEAD, discarding changes
git co master bar.txt           checkout file from another branch, overwriting curr branch changes
git co HEAD -- */*.config       check out everything matching filename foo from HEAD
git reset                       reset HEAD to specified state (unstage changes). does not alter files
git reset --hard master         reset HEAD and make files identical to master
git clean                       remove untracked files from the current working dir
git clean -n                    show which files would be removed
git clean -f                    remove untracked files (not dirs)
git rm foo                      remove local and remove remote on next push
git rm --cached foo.txt         retain local and remove remote on next push - this will delete foo.txt for others who pull

// VIEWING HISTORY
git show -B -w <hash>           show changes that were made for a commit, ignoring whitespace
git show HEAD~:./dir/file       show output of the file in dir/ as of last commit before HEAD
git log --follow foo.txt        see commit history of file
git log --follow -p foo.txt     see commit history and patch diff (code changes) of file
git log --first-parent master   see commits made to master
git log --author="Alex"         see commits made by Alex (regex can be used for the name)
git log --first-parent          see commits for current branch and then parent and its commits
git grep -I <pattern>           search files for pattern (-I excludes binaries)
git grep <pattern> -- *.h *.cpp grep through only .h and .cpp files
git diff                        show all changes made (but not necessarily added)
git diff --cached               show diff of staged, uncommitted changes
git diff master -- foo.txt      diff between your file (staged or not) and the version in master
git diff --name-status A..B     name and status of files that differ between branch A and head of B
git diff --name-status A...B    name and status of files that differ between branches A and B since they diverged
git diff master..               view diff between master and current unstaged changes
git diff -B -w --shortstat A..B just list num of files changed, insertions and deletions between commits
git rev-list --all | xargs git grep <expression>    search history for an expression
git log -S 'Needle' -p                              another way to search history to see when a string was added or removed, with patch
git log -S 'Needle' --source --all                  search all branches
																										or search with regex:
git log -G "^(\s)*function foo[(][)](\s)*{$" --source --all

git commit                      prep files to be committed
git commit foo                  revert to commit foo


// RESET RENAMELIMIT:
git config merge.renameLimit 999999
git config --unset merge.renameLimit


// git pull is unable to resolve references: error: Ref refs/remotes/origin/{branch} is at <hash1> but expected <hash2>
git gc --prune=now              clean up and optimize local repo, removing loose objects of any age (now)
git remote prune origin         delete all stale remote tracking branches under origin (remove local branches that are no longer in remote origin)
// actually try this:
git update-ref -d refs/remotes/origin/<branch_thats_barfing>


// CLEAN UP FILES
git clean -n -d                 see which files would be removed
git clean -f -d                 remove all untracked files that are not in .gitignore


// BUNDLE BUNDLE WHO'S YOUR BUNDLE?
// create:
git bundle create my-repo.bundle HEAD master
// extract:
git clone my-repo.bundle <optional dir>


### QUICK GUIDE

* make a new repo:

```
git init
git remote add origin https://github.com/adnammit/yourRepo.git
git add .
git commit -m "initialized repo"
git push origin master
```

* working with an existing repo:

```
git pull origin master
git add .
git commit -m "updates foo"
git push origin master
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


### SELECTIVE MERGING
* say you have a bunch of commits on a feature branch (`f3`) that are for three different issues. You want to group these changes into three commits when you merge into `master` so you can properly document them. how to do?

```bash
git co f3
git rebase master               # make sure feature is up to date - useful for diffing later
git co hotfix                   # check out some intermediary branch
git reset --hard master         # make sure it's the same as master
git cherry-pick <SHA1>          # cherry pick all your commits for one issue onto hotfix from f3
																#   in the order you want to apply them
git reset HEAD~n                # where n is the # of commits, unstages all those cherrypicks
git add -A                      # you may need to do this if there is a new file
git commit -a                   # write your lovely message for the one commit
git co master
git merge hotfix
# repeat for all issues until all commits in f3 have been merged into master
git diff f3..hotfix             # make sure you didn't forget anything
git push
```


### STASHING
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

### FIXUP
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



### GOTCHAS
* git might not see casing changes on files as an actual change, so you might need to explicitly "move" the file
		```bash
				git mv duck.ts Duck.ts
		```

### AMEND A COMMIT
* so say you're doing some work and you realize that you need to make a change to your last commit (make a small change to a file, say)
		```c
				//you can reset and re-commit:
				git reset HEAD~
				git aa
				git cm
				//or you can alter the commit:
				git aa
				git commit --amend
				<alter commit message if needed, save quit>
		```

### OMIT A FILE FROM A COMMIT
* use `update-index` to preserve local changes to something like a config file that points to your sandbox db -- you have changes that are just for you that you don't want to push
		```c
				// ignore changes to this file for now:
				git update-index --assume-unchanged foo.ts
				// once you're done with the file and want to reassimilate it into version control:
				git update-index --no-assume-unchanged foo.ts
		```


### RECOVERING LOST WORK WITH REFLOG
* the reflog contains a history of all recent changes
* say you did a `git reset --hard` but now you need those changes -- `reflog` has your back
* using `reflog` you can view commits that are not visible with `git log`
* suppose you were doing some work on `f1`, switched to master but then for some reason your commits on `f1` are gone!
		```bash
				# viewing the reflog, we can see missing commits foo, bar and baz
				$ git reflog
				cf42fa2... HEAD@{84}: checkout: moving to master
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
		- `reflog` only works with changes that were committed
		- `reflog` is yours and yours alone -- it can't help you with another person's uncommitted, unpushed work

### .GITIGNORE
* frequently there will be files in your git folder that you don’t want on the interweb, or that are unnecessary/inapplicable to the repo.
* use .gitignore to tell git which files to leave alone
* don’t gitignore .gitignore -- it will automatically prevent other contributors to the repo from polluting the repo with the same files you’re omitting
* Note that ignores that you don't want to share, e.g. specific configuration files for an IDE that only you use, can be ignored using .git/info/exclude. The format of this file is the same as .gitignore, but this file will never be committed
* to exclude everything in a directory *except* some file or files:
		```
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


### .GITATTRIBUTES
* `.gitattributes` can be configured to contain some useful instructions about how to treat your version-controlled content
* `export-ignore` tells git to ignore certain files/folders when someone downloads your repository
* `text-auto` normalizes line-endings to use LF
* `eol` specify line ending (say, for just one file or filename pattern)
* `binary`/`text` specifies how to treat files when commands like `git co`, `git add`, `git commit` and `git merge` are run
		- `text` enables end-of-line normalization
* `merge` strategy: specify default merge strategies for different files
* `-diff` do not merge these files
		```shell
				# put it all together:
				src/foo.php text eol=crlf       # treat foo.php as text and make sure it has 'crlf' line endings
				*.jpg binary                    # always treat jpg files as binary
				package-lock.json merge=ours    # use ours when merge conflicts arrive
				# do not try to merge these files:
				yarn.lock -diff
				public/build/js/*.js -diff
				public/build/css/*.css -diff
		```

### REMOVING FILES FROM GIT
* remove a local file from the directory
 ```
		$ git rm foo
		$ git commit -m "removed foo"
```
* remove the file only from the Git repository and not remove it from the filesystem:
```
		$ git rm --cached foo
		$ git commit -m "removed foo"   //only need to do this if the file was pushed
```


### CLEANSING SENSITIVE INFORMATION
* use bfg repo cleaner: https://rtyley.github.io/bfg-repo-cleaner/
* steps:
		- clone repo using `--mirror`:
		- make changes to the repo
		- cd into repo, call reflog, gc and push
		```
				$ git clone --mirror git://example.com/my-repo.git
				$ java -jar bfg.jar --delete-folders .git --delete-files .git  --no-blob-protection  my-repo.git
				$ cd my-repo.git
				$ git reflog expire --expire=now --all && git gc --prune=now --aggressive
				$ git push
		```
### FORMATTING
* you can use various options to format data about commits and files

#### DATES
* format options include:
		%cd: committer date (format respects --date= option)
		%cD: committer date, RFC2822 style
		%cr: committer date, relative
		%ct: committer date, UNIX timestamp
		%ci: committer date, ISO 8601-like format
		%cI: committer date, strict ISO 8601 format
* use `--format="%cd" --date=short` to easily get YYYY-MM-DD


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

