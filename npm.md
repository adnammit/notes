# Node Package Manager (NPM)
* npm is a package manager for javascript -- it started as a package manager for node.js (hosting CommonJS modules) but has since expanded to include front-end packages (ES Modules)
* npm is wonderful because before it came along, we had to download and install packages manually
	* for browser applications, this meant importing each individual script via tags in html, in the correct order according to their dependencies
	* when the version changed, we had to go through and change the version number in each script tag

## Node Version Manager (NVM)
* first let's talk about `nvm`, the package manager for your package manager
* download and install nvm -- then you can install, uninstall and use different versions of npm more flexibly
```sh
	# view downloaded versions
	nvm ls/list                 
	# view versions available to download
	nvm ls-remote               
	# install a specific version
	nvm install 10.5.0          
	# install the latest version
	nvm install latest          
	# you gotta say "use" if you actually want to use it -- just installing won't work
	nvm use 10.5.0              
	# set as default
	nvm alias default 10.5.0    
	# see what's available and select lts version if desired
	nvm list available			
```

## Node Package Execute (NPX)
* `npx` is a tool that comes with npm 5.2+ and higher
* npx allows you to run node packages without installing them globally, so instead of doing `npm install -g create-react-app` and then `create-react-app my-app`, you can just do `npx create-react-app my-app`


## NPM Usage
* npm is the default package manager for Node.js
* some npm functionality:
```sh
	# GENERAL
	# display version
	npm -v
	# search for packages like 'foo'
	npm search foo
	# view available versions of foo, optionally in json format
	npm view foo versions (--json)
	# view installed packages for the local project
	npm ls
	# view globally installed packages (depth=0: don't show dependencies)
	npm list -g --depth=0
	# install a package globally for use on the command line or in build scripts
	npm i -g <package>

	# PROJECTS
	# install all packages for a local project -- execute in dir containing package.json
	npm i
	# install a package locally: package is required to work in production
	npm i <package>
	# same as `npm i <package>`
	npm i <package> --save
	# install a package locally: package is only required for development
	npm i <package> --save-dev
	# install a specific version of a given package
	npm i [package]@[version]

	# UPDATE
	# update all packages in the local project -- follow with `npm i`
	npx npm-check-updates -u

	# MANAGEMENT
	# clear cache
	npm cache clean
	# remove unneeded packages
	npm prune
```
* should I install globally or locally?
	* if you're installing something that you want to use in your program, using `require` or `import`, then install it locally, at the root of your project.
	* if you're installing something that you want to use in your shell, on the command line or something, install it globally, so that its binaries end up in your PATH environment variable.
	* sometimes you will want both. seem excessive? don't worry - the space usage isn't significant
	* you can also symlink to the global install (or `npm link`?) but just installing it in both places is preferable
* notes on installing packages with npm:
	* is npm installing in `..` rather than your `pwd`? you might need to `$ mkdir node_modules` first so npm doesn't get distracted and try to walk up the folder structure
* **--save** vs **--save-dev**
	* as of NPM 5, `--save` is no longer necessary -- it's the default behavior, so really our choice is either `npm i` or `npm i --save-dev`
	* `npm i` installs a core dependency that is needed for the app to run in any context (dev or production)
	* `npm i --save-dev` installs a dependency that is only needed for development purposes, like testing or building the app -- examples include testing frameworks, build tools, and linters

## NPM Configuration
* view your configuration with
  `npm config list`
* npm uses a default public registry for its packages but you can specify other private registries
* use an `.npmrc` file (either globally in your user's home dir or in the install location) to specify what registry to use for a given domain:
	```
		@turtle:registry=http://proget.turtle.net/
		@walrus:registry=http://proget.walrus.net/
	```
* you can also specify registry on a command-level basis
	`npm install --registry=https://some.registry.url`

## Using package.json
* why use `package.json`? using `package.json` allows you to record your dependencies for webpack and babel and install them again with `npm install`
* you can define a "start" script so you don't even have to type out `webpack-dev-server etc` -- just do `npm start`
