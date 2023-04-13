
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
		tim corey
		educative.io
	- env: vscode, extensions
	- dotfiles
	- fonts
	- stack
	- tools
		sourcegraph
		postman
		f.lux
		lucidcharts
* continue to decouple git-controlled repos from Dropbox


## Fresh Install
Stuff to set up for a new computer. First, run [this script](https://github.com/adnammit/config/blob/master/bin/setup_all_the_things.sh)

### Dev Stuff
* vscode
* vscode extensions - look and editor
	* Atom Keymap
	* Block Travel
	* macros
	* One Dark Pro
	* Material Icon Theme
* vscode extensions - tooling
	* Typescript Vue Plugin
	* Vue Language Features
	* SQL Server (mssql)
	* docker
	* LaTeX Workshop
* git
* fonts: firacode, jetbrainsmono, operatormono
* cygwin -> Windows Terminal
* node/nvm
* npm global install:
	- vue
	- vue cli
	- corepack (what is?)
	- typescript
	- yarn
* docker
* pgadmin/postgres
* dotnet sdks
* texworks
* postman
* LICEcap: screen record to gif
* teensy
* visual studio
* visual studio extensions:
	- VSBlockJumper

### Regular Stuff
* fancyZones - like uber window snap (install via microsoft store - powertoys package)
* [synergy](https://symless.com/synergy) -- or logitech flow, or mouse without borders
* slack
* signal
* f.lux -- turn off
* spotify
* dropbox
* steam (look up what dirs to copy)
* some hardware monitor
* garmin express/basecamp?
* vlc
* winamp


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


