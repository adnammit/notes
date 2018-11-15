# I SHELL YOU SHELL WE SHELL

## GENERAL INFO

```
.bashrc             for interactive, non-login shells
.bash_profile       for login shells
.bash_history       list of your bash commands
$ history           spits out the contents of .bash_history in a nice, legible format
```


## ITERM:
```
^w                          close window
^d                          split pane vertically
^shift+d                    split pane horizontally
^[ and ^]                   toggle between panes
^t                          new tab
^#                          go to tab #
^,                          prefs
```

## CYGWIN
```
apt-cyg install foo         install package foo
apt-cyg show bar            display info about a package
apt-cyg download foo        download but do not install foo
apt-cyg update              get a fresh copy of the master package list
apt-cyg list                list all installed packages
apt-cyg list baz            list all installed packages with names that match 'baz'
apt-cyg listall baz         find all packages in the master list with names that match 'baz'
```

## LINUX-SPECIFIC

```
apt-get 	package manager for debian-linux
apt list --installed	list all installed pkgs
sudo apt-get install foo	install package foo
sudo apt-get install *foo*	install all packages matching ‘*foo*’
sudo apt-get update	resychronize the package index files from the sources listed in /etc/apt/sources.list w/out actually changing installed packages
sudo apt-get upgrade	looks at the updated index of packages and upgrades necessary pkgs
sudo apt-get dist-upgrade	more sophisticated upgrade which intelligently resolves dependencies
apt-cache pkgnames <foo>	list all packages (optionally those matching ‘foo’)
apt-cache search <foo>	display available packages (optionally matching ‘foo’)
apt-cache show netcat	display details about netcat
apt-cache showpkg vsftpd	display dependencies for vsftpd
apt-cache stats	display statistics about the cache
sudo apt-get clean	clear up disk space by removing .deb files
sudo apt-get remove foo	remove package foo but leave config intact
sudo apt-get purge foo	remove package foo including foo’s config files (watch out!)
sudo apt-get autoremove	remove any dependencies that are no longer needed
```


## BASHRC, BASH_PROFILE, ALIASES, FUNCTIONS AND SCRIPTING
* .bashrc vs .bash_profile vs .profile:
  - the .bashrc is bash's "interactive-mode-only" config file
  - generally your .bashrc is for an interactive shell, whereas the .bash_profile is for non-interactive shells
  - for mac, X11 will look at your .bashrc while a "regular" Terminal will look at .bash_profile
  - However, if you add the following to your .bash_profile, you can then move everything into your .bashrc file so as to consolidate everything into one place instead of two:
  ```bash
  if [ -f $HOME/.bashrc ]; then
      source $HOME/.bashrc
  fi
  ```
* .profile should contain all your PATH variables and friends, and should be sh-friendly only (no bash)
* aliases:
   - aliases should be used when you need to simply modify the behavior of a command
          `alias ls="ls --color=auto"`
   - you can call a command without it's alias by writing it in all caps:
          `$ LS     // calls the generic 'ls' command with no arguments of the alias`
* functions:
   - functions are a bit more complex than aliases and may take arguments, but are not things you
     would necessarily use on your own
   - if you get a lot of functions or want to modularize them (use the same functions with different
     .bashrc configurations, etc) you can put them in their own hidden dir:
     ```bash
     if [ -d ~/.bash_functions ]; then
         for file in ~/.bash_functions/*; do
             . "$file"
         done
     fi
     ```
   - can change the shell's environment
* aliases and functions stored in .bashrc:
   - if you only want the interactive shell to use your code, putting an alias or function in
     the .bashrc is a good bet
   - they can only be used by the current shell and are kept in the shell's memory
       * if it's something you use frequently, it may be more efficient to keep it here - scripts are re-read from the disk every time they're invoked
	   * on the other hand, if you don't use it frequently and it's large, no need to have it hogging your memory
   - no separate process is needed to run them
* scripts:
   - scripts should stand on their own. they may be re-used, or be broadly applicable
   - necessary if other programs beside your shell need to use it
   - scripts may be invoked in more ways: they may be passed as an argument to the interpreter or invoked directly as an executable
   - scripts run as their own process, with their own environment that will not effect the shell's
       * however if the script is sourced with "." or "source" it will use the shell's env and can modify it
* if you get an error like `-bash: '\r': command not found` try running the affected file through `dos2unix`:
        $ dos2unix foo.sh
    - caution: this will alter your file, so make sure there's a backup



## GENERAL COMMANDS:

|command|notes|
|:--|:--|
|`cd`|use '$ cd -' to cd into the previous directory|
|`!!`|repeat the last command|
|`!xyz`|repeat the last command that started with 'xyz'|
|`!xyz:p`|print the last command that started with 'xyz' (then do !! to execute)|
|`bg/fg`|bring a suspended process to the background/foreground|
|`[command] $_`|take the last arg of the last command (or the command before it if the last one had no args) and use it as the arg for the current command|




```
cd                              use '$ cd -' to cd into the previous directory

!!                              repeat the last command
!xyz                            repeat the last command that started with 'xyz'
!xyz:p                          print the last command that started with 'xyz' (then do !! to execute)

bg/fg                           bring a suspended process to the background/foreground

[command] $_                    take the last arg of the last command (or the command before it if the last one had no args) and use it as the arg for the current command

rdiff                           diff RCS versions
diff file1 file2
    -y                          view output in two columns
    -y -W 200                   view side-by-side and make the columns 200 chars wide
    -Nr dir1/ dir2/             diff entire directory: displays diffs in individual files and what files are only in one dir or the other
    -rq dir1/ dir2/             only view differences in what files are in each dir, not file contents
    -Bw                         ignore all (w)hitespace and (B)lank lines

rm                              remove
	-i                          confirm deletion of each file individually
	-f                          delete without asking (overrides '-i' alias)
	-rf                         use with discretion! forcefully (without prompting) recursively remove all

cp -r foo/. bar/                copy the contents of foo (including directories and hidden files) to bar

rsync -a foo dir1/dir2/dir3/    copy file foo into the path, creating folders as needed. (don't forget closing /)

mkdir -p                        make a directory. -p: no error if existing

man pages:
        / ?                     search forward/backward within an open man page
        n/N                     move forward/back through search results
        man -K "foo"            search across all man pages

type [command]                  display the exact command being executed

alias [alias]                   arg is optional -- with no args, displays all aliases and their commands

[LS]                            windows: type command in all caps to override whatever you aliased it to

\ls                             linux: escape your command to override whatever you aliased it to

which [executable]              displays the full path of shell command

[command] > foo.txt             write the output of command to a file. create file if it doesn't exist.
                                Overwrites previous content of file.

echo foo >> test.txt            append 'foo' to test.txt. create test.txt if it doesn't already exist.

[command] | tee foo.txt         write the output of command to file and display output

tar                             make or unzip a zip file
tar cvf all.tar somedir/*       zips the contents of somedir into all.tar
tar tvf all.tar                 view the contents of all.tar
tar xvf all.tar                 unzips the contents of all.tar into dir all/
                                !!! this will overwrite anything in an existing all/ dir
tar xvf all.tar -C my_dir/      extract all.tar into directory                                

sed                             stream editor for filtering and transforming text
sed ‘s/night/day/’ foo.txt      replace instances of ‘night’ with ‘day’
sed ‘s|night|day|’ foo.txt      can also be pipe-delimited -- other delimiters work too

sort -u foo.txt                 print a list of unique sorted lines in foo.txt

./                              execute


exec foo                        replace the shell with the given command.
                                use '$ exec bash' to reload your config files and replace your current instance of bash with a new one (but maybe not -- not all config files are loaded w/ exec bash)

close your exec command with ; or + depending on the behavior you want:
\; -- execute the command once per argument
\+ -- pass in all args

			      $ find -name "*oo*" -exec echo {} \+
			      > soon balloon pantaloon

			      $ find -name "*oo*" -exec echo {} \;
			      > soon
			      > balloon
			      > pantaloon

```


## SYS ADMIN-Y STUFF
```
env                             set and print environment

file                            determine file type

mount                           with no args, view information about the filesystem hierarchy

df                              display free disk space

du                              estimate file space usage
                                for a nice, useful output try '$ du -sh * | sort -h'

top                             view processes

top -u [username]               view processes for one user

htop                            top with cool graphics!

pgrep                           find processes

ps aux                          view processes on the *system* (aside from shell internal processes)
                                (aux == all users)

ps aux | less                   view in less

ps -u [username]                view processes for one user

ps -elf                         another way

jobs                            display jobs that the *shell* is currently managing

kill [PID]                      attempt to stop process with a given PID by sending TERM signal
    -KILL                       attempt to kill process with KILL signal
	-9                          same as -KILL
	-l                          list all possible signals to send

killall firefox                 kill all instances of firefox

chmod                           change file modes or access control lists - see extensive instructions below
chown                           change owner and group

uptime                          prints how long the system has been running

which foo                       displays the origin of foo (whether it is in /usr/bin, is an alias, etc)

sudo find / -size +2000000 -print
                                find all files larger than 2 gigs

find . -user ryman.amanda       find all files owned by user

traceroute google.com           show an IP network's route packet trace to the network host

```




## SEARCHING:
```
find    -[i]name	by name [case insensitive]
    	-[i]regex	search using [case insensitive] regexes
    	-perm		/u+w
    	-type		f=files; d=dirs; l=symlinks
    	-size
    	-user		files belonging to a particular user
    	-group		files belonging to a particular group
     	-maxdepth n	limit search to depth of n. n=1: search this dir only

grep
	-l		show only files containing matches (not the matching text)
	-i	    ignore case
	-n 	    display line number of match
    -m1     only display the first match in each file
	-q		quiet
	-r		recursive. follows sym links
    -F      literal. helpful when searching for expressions containing '.'
	--color	the colors, duke!
	-A6		display the 6 lines after the expression is found
	-B6		display the 6 lines before the expression is found
	-C6		display the 6 lines before and after the expression is found

	Example: find log files that contain 'error' and 'project_propagate' in the same line
		   $ find -name '*log*' | xargs grep -i error | grep -i project_propagate
    OR: (find 'document' followed by 'content' on the same line)
           $ find -name '*.js' | xargs grep -E 'document.*content' --color

bzgrep
	-e [pattern]	protect patterns beginning with -
	-r 		recursive. example searching recursively from pwd for 'pattern':
				   $bzgrep -r pattern .

egrep	    	 	searches for extended regular expressions
fgrep	    		faster but can only handle fixed patterns (cannot handle regex)
pgrep	    		search processes
zgrep	    		zgrep, zegrep and zfgrep act like grep, egrep and fgrep respectively
zegrep	    		but accept compressed input files
zfgrep
```


## READERS
```

less	       		simple text viewer
	+F		view page dynamically

more			view text one page at a time (pretty primitive)

cat			concatenate file to stdout

most

head
	-30		view first 30 lines of the file

tail
	-30		view last 30 lines of the file
	-f		view dynamic content (less +F is probably better)

bzless			view bzfiles
```




## CIRCUMFLEX HATS

* use circumflex hats (^) to re-execute the previous command with an alteration.
* you might want to do this if you typed a long command with an error, or if you need to repeat self-similar commands
* example:
```bash
    $ emacs .xdefaults &        // creates a new file
	$ ^x^X                      // opens the correct file
```
* or how about
```bash
    $ cp foo.txt my/very/long/dir/path/
    $ ^foo^bar				   // copy both foo and bar.txt to the same complex dir
```


## REGEXES

* Vocabulary:
    - Literal: literally the string we want to find, the ‘ing’ in ‘finger’
    - Metacharacter: a special character whose meaning is not taken literally (the circumflex hat ^ for example)
    - Target string: the string in which we are searching for our pattern
    - Search (regular) expression: combination of literals and metacharacters used to find our match
    - Escape sequence: used to indicate that we want to use the literal value of one of our metacharacters. The escape metacharacter is \
* Metacharacters:
    - \[\*\] 	match anything inside brackets for one character position, once and only once
    - \- range separator indicating a range, such as [0-9]. Can include multiple ranges, such as [0-9A-C]
    - ^	the circumflex inside square brackets selects the complement of its associated literal
        ```
            [^fF]       not f or F
            [abc]       only a, b or c
            (abc|xyz)   abc or xyz   
        ```
* Special characters:
    - `\s` any whitespace
    - `\S` any non-whitespace
    - `\d` any digit
    - `\D` any non-digit
    - `\w` any alphanumeric char
    - `\W` any non-alphanumeric char
    - `.`  any character
        ```
            ^\s*Foo     lines that start with 'Foo', ignoring any amount of whitespace
        ```


## SHORTCUTS
```
ctrl+z			suspend a process with SIGSTOP which cannot be intercepted by the process

ctrl+c			kill a process with SIGINT, which can be intercepted by the process (which allows it to clean
			or override the signal)

ctrl+a			move cursor to the beginning of your command

ctrl+e			move cursor to the end of your command

ctrl+u			delete everything before the cursor and put it in a buffer

ctrl+k			delete everything after the cursor and put it in a buffer

ctrl+w			delete a word

ctrl+y			yank (paste) the buffer contents at the cursor

ctrl+r			search back through bash_history for a match (eg $ ^r grep)
			hitting ^r again will cycle through the results
```

## LINKS
```

ln /dir/file link_name	creates a hard link to file. note that link_name cannot contain a dir path.
  	     		delete hard link with rm. data will still be in dir/file.
			data will still be accessible through link_name after deleting dir/file.

ln -s <source> <dest>	create a (soft) symbolic link. dest may contain a path.
     	       		edeleting the link will leave the source data intact.
			deleting the source data leaves the link broken.

unlink foo		like rm for symlinks, w/out the implication of deleting its 'contents'
```



## CHMODDING!

* format:
```
   [d][r][w][x][r][w][x][r][w][x]
   [directory?][user: rwx][group: rwx][all: rwx]
```
* users:
    - u: user. owner of the file
    - g: group. users who are members of the file's group
    - o: others. users who are neither.
    - a: all. everyone.
* operators:
    - +
    - -
    - =
* modes:
    - r: read or list
    - w: write
    - x: execute or recurse a directory tree
    - X: special execute.
    - s: setuid/gid
    - t: sticky
* view permissions with `ls -l filename.txt`
* examples:
```
   chmod a+w filename.txt	everyone can write
   chmod ug=rx reference.txt	user and group can read and execute only
   chmod a-w file.txt		no one can write
   chmod +x file.txt		everyone can execute
```   
* directories:
    - the group can add files:
```    
   ls -ld shared_dir  -->   drwxr-xr-x
   chmod g+w shared_dir --> drwxrwxr-x
```
