# TODO
* **SDK vs Runtime**
* write a bash func that replaces `git push`
	* check to see if on branch `main` or `master`
	* if so, force confirm, else just do it
* finish setting up database
	* convert `media` to regular sql script
	* seed the aws database
* set up your upNext service
* do something about all your env secrets?
* fix your website, dude
	* move core site to amplify
	* actually just create a whole new site
	* figure out what content you want to include?
		* upnext.amandaryman.com
		* cowpokes.amandaryman.com
		* checkbook.amandaryman.com
		* amandaryman.com
			* /uses
			* about
			* work
			* notes
* make a monorepo for permit project and upnext - figure out how to deploy
* check out SvelteKit/Vite
* write a script to periodically update stuff like git, powershell, docker
* keep an eye on Bun, a js runtime that may contend with Nodejs: with native ts support, written in a C-like language, built in compiler rather than webpack, it can handle many more requests than node in prelim testing
* check out browserless.io, use for debugging, testing, snapshots, etc
* try fakeapi to easily mock services
* make a /uses
	* sites i like
		* https://restfulapi.net/resource-naming/
		* https://materialdesignicons.com/
		* https://eloquentjavascript.net/
		* lucid charts
		* name that color!
		* tim corey
		* educative.io
	* env: vscode, extensions
	* dotfiles
	* fonts
	* stack
	* tools
		* sourcegraph
		* postman
		* f.lux
		* lucidcharts
* learn:
	* hangfire, rabbitmq
	* kubernetes/docker-compose
	* snap ins
	* entity
	* azure/t-sql
	* sql review
	* aws/ecs
	* google cloud
	* scala, haskell
	* maths: fractions, ratio, proportions, linear equations

## Fun Things To Do And Know
* learn more about CQRS
* check out fastcomet for webhosting
* render/netlify for fast back-end prototyping
* learn about cloudformation (better alternative to terraform)
* make an agent-based model to graphically display the growth of a search tree
* probabilistic data structures
* learn more about security and authentication
* get involved with open source
* learn more about the DOM
* use [codepen](https://codepen.io/) to practice front end coding
* try making a game with Unity (C#)
* kaggle: data sets to play with
* high charts: make cool charts in your browser
* use draw.io for diagramming

# Bash Stuff
* make the git equivalent of `sall` (grep for multiple expressions)
* learn parsing bash opts more betterly
* go through scripts and clean up the following:
	* braces are used for variable expansion
		* they are required when referencing an element in an array
		* they are necessary for expansion operations
		* used in expanding positional parameters past 9
	* quotation marks
		* can be used to preserve a string containing whitespace
		* quotes are also used to prevent errors in the case of evaluating a variable that is null
		* to avoid having to make quotes all over the damn place, you can use [[ ]] for tests
