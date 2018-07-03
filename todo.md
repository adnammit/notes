
# TO DO

#### F1 >> dev
\>> PROJECT AND REQUEST LAUNCH STUFF

#### F2 >> test
\>> PF 77: MAKE DATED NOTES TOP POSTING IN WEB AND IN TOOL >> hold
\>> PF 71: CREATE ACTION TO COPY FIELD FROM FORM TO FORM >> test

#### F3 >> reset
\>> JHS ERROR DEBUGGING

#### F4 >> dev
\>> 599: CHANGE BIRT FORM NAME TO DATA

#### F5 >> test
\>> RESPECT REQUIRED FIELDS IN TOOL

#### visn-perms >> that visn transfer thing -- does megan want this?

#### file-cleanup
\>> remove old maint files for ashland, sheridan and jhs gov req -- check in next week



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
```

### GENERAL STUFF
* continue to decouple git-controlled repos from Dropbox

### BASH STUFF
* for update_pl func: return fail status to the caller
* script to reset branch and rcp
* get git completion to work
* make the git equivalent of `sall` (grep for multiple expressions)
* script to roll all packages for orgs in a repo?
* learn parsing bash opts more betterly

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

### WEB DEV STUFF
* figure out how to stop `webpack-dev-server` gracefully ('stop' script in webpack config?)
* babel presets are all over the place (.babelrc, package.json, webpack config) -- where should they actually go?
* check out react spring for animation
* check out react routing
* check out enhanced object literals


ssh://dev-src.portland.perflogic.com:20022/src/git/main.git
