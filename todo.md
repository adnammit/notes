
# TO DO

#### F1 >> dev
\>> PROJECT AND REQUEST LAUNCH STUFF -- initial

#### F2 >> dev
\>> PROJECT AND REQUEST LAUNCH STUFF -- recent

#### F3 >> dev -- BRANCHED OFF F2
\>> PROJECT AND REQUEST LAUNCH STUFF + editable forms

#### F4 >> dev
\>> JHS 587: ASYNC EMAIL

#### F5 >> reset -- no repo site
\>>



#### file-cleanup
\>> remove old maint files for ashland, sheridan and jhs gov req
\>> do another lf cleanse

**run email repair script on jhs (and anyone else?)**



### NEW STUFF:
```
testwc: display everything i did for test
diws: diff ignoring whitespace, newlines and comments
giff <mh>: git diff <from master to HEAD>
giffns <mh>: git namestatus diff <from master to HEAD>
giffm: giff between master and current changes (committed or no), file or whole repo
gish: git show <commit> <filepath>, ignoring ws
linkorg [org]: alias for copy_org, links org's data to alr repo site
find_pf_clients: list all process flow clients
sync_pf_clients: copy the data for all pf clients
scd [sync_client_data]: copy data for multiple clients
    $ sync_client_data jhs epsg wellmont --link alr
diff_dirs: takes 2 dirs as args, diff all files between them
rcp: roll_changed_pkgs
grst: grep in files named *state.dat
```

### ATOM NEW STUFF:
```
C-l                         select the current line
C-j                         join next line to end of current line
C-E                         select to end of line
C-A                         select to beginning of line
C-F                         find in project
C-T                         show in tree view
C-D                         duplicate current line
C-click                     duplicate cursor at click point
C-shift-p ["add..."]        add new file to current folder
C-u C-0                     unfold all
C-u C-n                     fold all at indent level n
C-alt-m                     select to matching tag
C-pageup                    move to previous pane
C-pagedown                  move to next pane
```

### GENERAL STUFF
* continue to decouple git-controlled repos from Dropbox

### BASH STUFF
* for update_pl func: we check for success but what happens when git pull/merge fails?
* get git completion to work
* make the git equivalent of `sall` (grep for multiple expressions)
* learn parsing bash opts more betterly
* go through scripts and clean up the following:
    * braces are used for variable expansion
        - they are required when referencing an element in an array
        - they are necessary for expansion operations
        - used in expanding positional parameters past 9
    * quotation marks
        - can be used to preserve a string containing whitespace
        - quotes are also used to prevent errors in the case of evaluating a variable that is null
        - to avoid having to make quotes all over the damn place, you can use [[ ]] for tests

### ATOM TO DO:
* write a thing to `git lock` from atom
* find and replace sucks -- try multiple cursor selection
* autocomplete sucks
* use swirly font for comments
* pirtle says: use apm to install packages through DOS (command prompt) rather than cygwin
* is there a right-click "buffer manager" like with emacs? or a faster way to navigate through open files?
* check out atom tags and bookmarks

### GIT
* what is all this `revs` shit?
* what is up with all this git diff shit -- are there helpful aliases?
* what is up with all this git log shit -- are there helpful aliases?
* new stuff:
    ```
    git co -        checkout last branch
    ```

### WEB DEV STUFF
* figure out how to stop `webpack-dev-server` gracefully ('stop' script in webpack config?)
* babel presets are all over the place (.babelrc, package.json, webpack config) -- where should they actually go?
* check out react spring for animation
* check out react routing
* check out enhanced object literals


ssh://dev-src.portland.perflogic.com:20022/src/git/main.git
