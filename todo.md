
# TO DO

#### F1 >> dev
\>> f3 + editable form data and docs

#### F2 >> reset
\>> reset from master

#### F3 >> dev -- BRANCHED OFF F2
\>> PROJECT AND REQUEST LAUNCH STUFF + editable forms

#### F4 >> dev
\>> JHS 587: ASYNC EMAIL

#### F5 >> reset -- no repo site
\>>

#### CHECKLAUNCH -- contains the unsquashed commits that f1 is based off of (e3d8678434) 
#### file-cleanup
\>> remove old maint files for ashland, sheridan and jhs gov req
\>> add skip_logging_emails to org_settings
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
```c
// EDIT
C-l                                 select the current line
C-j                                 join next line to end of current line
C-E                                 select to end of line
C-A                                 select to beginning of line
C-d                                 select current word
C-D                                 duplicate current line

// NAVIGATE
C-m                                 jump to matching tag
C-alt-m                             select to matching tag
C-pageup                            move to previous pane
C-pagedown                          move to next pane
alt-left/right                      jump to beginning/end of the line
C-click                             duplicate cursor at click point
alt-up/down                         create new cursor up or down
c-a-shift-up/down/left/right        move last cursor up/down/left/right

// ETC
C-F                                 find in project
C-T                                 show in tree view
C-shift-p ["add..."]                add new file to current folder
C-alt-{                             fold all
C-alt-}                             unfold all
C-u C-0                             also unfold all
C-u C-n                             fold all at indent level n
alt-C                               plscript: clean-file
find -name 'foo.js' | xargs atom    open all files called 'foo.js' in atom
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
* use swirly font for comments
* pirtle says: use apm to install packages through DOS (command prompt) rather than cygwin
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
* check out browserless.io, use for debugging, testing, snapshots, etc

ssh://dev-src.portland.perflogic.com:20022/src/git/main.git
