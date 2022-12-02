
# TO DO
* keep an eye on Bun, a js runtime that may contend with Nodejs: with native ts support, written in a C-like language, built in compiler rather than webpack, it can handle many more requests than node in prelim testing
* check out browserless.io, use for debugging, testing, snapshots, etc
* make vscode sql better
	- configure connection
	- something like redgate search?
* go through config and clean up stuff you don't use/need anymore
* make a /uses
	- sites i like
		https://restfulapi.net/resource-naming/
		https://materialdesignicons.com/
		lucid charts
		name that color!
	- env: vscode, extensions
	- dotfiles
	- fonts
	- stack
	- tools
		sourcegraph
		postman

* continue to decouple git-controlled repos from Dropbox


### NEW STUFF:
* vs theme
* custom bash icon?
* get WSL2 some day

vue cli
webpack cli -- install --save-dev, not globally
stackify prefix
winptty
resharper
rider (vs alternative)
vnc viewer


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


