
# TO DO

#### F1 >> dev
\>> PF 75: ADD CHAR/SIZE LIMIT TO FREE TEXT/NUMBER

#### F2 >> testing
\>> PF 121: STATUS LIGHT ON WEBFORMS

#### F3 >> testing [epsg]
\>> PF 79: WEB FORM WIDTHS

#### F4 >> dev
\>> PPMLITE 415: OTHER OPTION DOES NOT SAVE FOR CHECKLISTS

#### F5 >> dev
\>> JHS 541: CHANGE LOG ASSIGNED TO STOPPED POPULATING CHANGE LOG

#### visn-perms >> that visn transfer thing -- does megan want this?

#### file-cleanup
\>> remove old maint files for ashland, sheridan and jhs gov req -- check in next week



### NEW STUFF:
```
testwc: display everything i did for test
diws: diff ignoring whitespace, newlines and comments
giff: git diff ignoring whitespace, newlines and comments
giffns: git diff ignoring whitespace, newlines and comments; show filenames only
linkorg [org]: alias for copy_org, links org's data to alr repo site
find_pf_clients: list all process flow clients
sync_pf_clients: copy the data for all pf clients
scd [sync_client_data]: copy data for multiple clients
    $ sync_client_data jhs epsg wellmont --link alr
update_dev: update and roll master and the curr feature
diff_dirs: takes 2 dirs as args, diff all files between them
rcp: roll_changed_pkgs
grst: grep in files named *state.dat
```
### ATOM NEW STUFF:
```
ctrl + m            jump to matching bracket/tag
ctrl + shift + f    find in project
ctrl + shift + t    show in tree view
```

### GENERAL STUFF
* continue to decouple git-controlled repos from Dropbox

### BASH STUFF
* get git completion to work
* make the git equivalent of `sall` (grep for multiple expressions)
* script to roll all packages for orgs in a repo?
* learn parsing bash opts more betterly

### ATOM TO DO:
* kill only kills the current line, not wrapped text -- try native ctrl-x
* find and replace sucks -- try multiple cursor selection
* autocomplete sucks
* use swirly font for comments
* pirtle says: use apm to install packages through DOS (command prompt) rather than cygwin
* is there a right-click "buffer manager" like with emacs?


### GIT
* what is all this `revs` shit?
* what is up with all this git diff shit -- are there helpful aliases?
* what is up with all this git log shit -- are there helpful aliases?
* set up key on github so you don't have to sign in for pushes

### NODE/REACT
* figure out how to stop `webpack-dev-server` gracefully ('stop' script in webpack config?)
* babel presets are all over the place (.babelrc, package.json, webpack config) -- where should they actually go?


### NEXT ROLL:
* ashland hasn't been rolled to -- do release procedure for #49
* blank users issue: script needs to be run on jhs & ahi

### RELEASE 2/22
* **RELEASE PROCESS FOR JHS:**
    - upload new form
    - remove macro replace flag
    - run pf scripts
    - run script to copy 'date' over to 'request_date'


### RELEASE 2/15
* run scripts for issue #42 (added validate params to "Submitted" states)
* baybluffs and epsg didn't get hit last week:
    - epsg
    - baybluffs


### RELEASE 2/8
* common
* process_flow
* web/maint

* run scripts for issue #42 (added validate params to "Submitted" states) and wellmont 184
    - jhs
    - wellmont


### CODE REVIEW IDEAS
* patch script for jhs `__maint__/client/jhs/`


ssh://dev-src.portland.perflogic.com:20022/src/git/main.git
