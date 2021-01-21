
# TO DO


### NEW STUFF:
* vs theme
* custom bash icon?
* get WSL2 some day
* write a script to create a new branch, run monsterbuilder and switcher

vue cli
webpack cli -- install --save-dev, not globally
stackify prefix
winpty
resharper
vnc viewer



### GENERAL STUFF
* continue to decouple git-controlled repos from Dropbox
* look over your env and make it less stupid/convoluted
     - don't source more than what you need
     - there might be build-in vars already for stuff like "program data" etc -- use those where possible
 * get calling vsstudio from the command line to work

### BASH STUFF
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


### WEB DEV STUFF
* figure out how to stop `webpack-dev-server` gracefully ('stop' script in webpack config?)
* babel presets are all over the place (.babelrc, package.json, webpack config) -- where should they actually go?
* check out react spring for animation
* check out react routing
* check out enhanced object literals
* check out browserless.io, use for debugging, testing, snapshots, etc
